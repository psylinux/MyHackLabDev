---
author: Marcos Azevedo
date: '2018-10-02T17:38:15-03:00'
linktitle: 'HowTo :: SSH access using a GPG key for authentication'
title: 'HowTo :: SSH access using a GPG key for authentication'
categories:
  - howto
  - ssh
  - GPG
tags:
  - howto
  - ssh
  - GPG
weight: 10
date updated: '2021-03-25T17:38:15-03:00'

---


## Preface
Many of us are familiar with Secure Shell (SSH), which allows us to connect to other systems using a key instead of a password. This guide will explain how to eliminate SSH keys and use a GNU Privacy Guard (GPG) subkey instead. Using GPG does not make your SSH connections more secure. SSH is a secure protocol, and SSH keys are secure. Instead, it makes certain forms of key distribution and backup management easier. It also will not change your workflow for using SSH. All commands will continue to work as you expect, except that you will no longer have SSH private keys and you will unlock your GPG key instead.

By having SSH authenticated by your GPG key, you will reduce the number of key files you need to secure and back up. This means that your key management hygiene still has to be good, which means choosing good passphrases and using appropriate key preservation strategies. Remember, you shouldn't back your private key up to the cloud!

Additionally, today SSH keys are distributed by hand and oftentimes directly. If you want to grant me access to a machine, you have to ask me for my SSH key. You may get lucky and find one posted on my website. However, you still have to decide if you trust my website. If I use a GPG key for SSH, you can select a known, good key for me using the GPG web of trust from a public keyserver. This is what [The Monkeysphere Project](https://monkeysphere.info/) is working on. Otherwise, nothing you do here affects the web of trust used for GPG encryption and signing.

## What is a GPG subkey?
A GPG key is actually a collection of keys. There is one primary key, which is typically used only for signing and certification. The suggested usage of GPG is to create a subkey for encryption. This subkey is a separate key that, for all intents and purposes, is signed by your primary key and transmitted at the same time. This practice allows you to revoke the encryption subkey on its own, such as if it becomes compromised, while keeping your primary key valid.
The important thing to realize is that a GPG key contains multiple keys. For backup and storage purposes, you can operate them as though they are one key, but when it is time to use a key, you can use them independently.

This exercise will use a subkey that has been created for authentication to complete SSH connections. This authentication subkey will completely replace the keypair you may have generated in the past with **ssh key-gen**. You can create as many of these as you want if you need multiple SSH keys.

## Create an authentication subkey
You should already have a GPG key. If you don't, read one of the many [fine tutorials](https://docs.fedoraproject.org/en-US/quick-docs/create-gpg-keys/) available on this topic. You will create the subkey by editing your existing key. You need to edit your key in expert mode to get access to the appropriate options.
The workflow adds a new key where you can choose its capabilities -- specifically, you want to toggle its capabilities to just have authentication. SSH typically uses a 2048-bit RSA key that does not expire (type 8 in the options below).
Below is an edited version of the workflow. This and all other commands were tested on Fedora 29.

```bash
$ gpg2 --expert --edit-key <KEY ID>

gpg> addkey

Please select what kind of key you want:
(3) DSA (sign only)
(4) RSA (sign only)
(5) Elgamal (encrypt only)
(6) RSA (encrypt only)
(7) DSA (set your own capabilities)
(8) RSA (set your own capabilities)
(10) ECC (sign only)
(11) ECC (set your own capabilities)
(12) ECC (encrypt only)
(13) Existing key
Your selection? 8

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Sign Encrypt

(S) Toggle the sign capability
(E) Toggle the encrypt capability
(A) Toggle the authenticate capability
(Q) Finished
Your selection? s
Your selection? e
Your selection? a

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Authenticate

(S) Toggle the sign capability
(E) Toggle the encrypt capability
(A) Toggle the authenticate capability
(Q) Finished
Your selection? q

RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048)
Requested keysize is 2048 bits

Please specify how long the key should be valid.
0 = key does not expire
<n> = key expires in n days
<n>w = key expires in n weeks
<n>m = key expires in n months
<n>y = key expires in n years
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y
Really create? (y/N) y
 

sec rsa4096/5B6C9AA9718B1298
created: 2019-03-21 expires: 2021-03-20 usage: SC
trust: ultimate validity: ultimate

ssb rsa4096/4CDB1F634D2FD7C
created: 2019-03-21 expires: 2021-03-20 usage: E

ssb rsa4096/136F0B4E3D42CF31
created: 2019-03-21 expires: never usage: A

[ultimate] (1). Marcos Azevedo (psylinux)

gpg> quit
Save changes? (y/N) y
```

## Enable the GPG subkey
When you use SSH, a program called **ssh-agent** is used to manage the keys. To use a GPG key, you'll use a similar program, **gpg-agent**, that manages GPG keys. To get **gpg-agent** to handle requests from SSH, you need to enable support by adding the line **enable-ssh-support** to the **~/.gnupg/gpg-agent.conf**.

```bash
$ cat .gnupg/gpg-agent.conf
enable-ssh-support
```
  
Optionally, you may want to pre-specify the keys to be used for SSH so you won't have to use **ssh-add** to load the keys. To do this, specify the keys in the **~/.gnupg/sshcontrol** file. The entries in this file are _keygrips -- internal identifiers **gpg-agent** uses to refer to keys. Unlike a key hash, a keygrip refers to both the public and private key. To find the keygrip, use **gpg2 -K --with-keygrip**, as shown below. Then add that line to the **sshcontrol** file.

```bash
$ gpg2 -K --with-keygrip
/home/psylinux/.gnupg/pubring.kbx
------------------------------
sec rsa2048 2019-03-21 [SC] [expires: 2021-03-20]
26C33AA7D4E4F7651C75AC218725AD32291EB131

Keygrip = 131E27EAC300658183D3E6B94B6D9AB1778A0291
uid [ultimate] Marcos Azevedo (psylinux)

ssb rsa2048 2019-03-21 [E] [expires: 2021-03-20]
Keygrip = 59A032B3BE1574BA0960DEDFB28FC26489383171

ssb rsa2048 2019-03-21 [A]
Keygrip = 2BE91C3C443AA29C3703D004587AD6DA9D7E40FC
```

Then write your corresponding **keygrip** into **sshcontrol** file
```bash
$ echo 2BE91C3C443AA29C3703D004587AD6DA9D7E40FC >> ~/.gnupg/sshcontrol
```

Last, you need to tell SSH how to access the **gpg-agent**. This is done by changing the value of the **SSH_AUTH_SOCK** environment variable. The following two lines, when added to your **~/.bashrc**, will ensure the variable is set correctly and that the agent is launched and ready for use.

```bash
$ cat ~/.bashrc

export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
gpgconf --launch gpg-agent

```

To continue, execute those commands in your current session.

## Share your SSH key
In order to use SSH, you need to share your public key with the remote host. You have two options. First, you can run **ssh-add -L** to list your public keys and copy it manually to the remote host. You can also use **ssh-copy-id**. From this perspective, nothing has changed.

## Congratulations!
You have now enabled SSH access using a GPG key for authentication! SSH will continue to work as expected, and the machines you are connecting to won't need any configuration changes. You've reduced the number of key files you need to manage and securely back up while simultaneously enabling the opportunity to take part in different forms of key distribution. Stay safe and practice good key hygiene!