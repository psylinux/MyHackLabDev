---
author: "Marcos Azevedo"
date: 2020-06-16
linktitle: "Cheat Sheet :: Linux Shell Tricks"
title: "Cheat Sheet :: Linux Shell Tricks"
categories: [ "cheatsheet", "exploitation-techniques" ]
tags: [ "shell", "bash", "linux" ]
weight: 10
---

# Linux Shell Tricks

## To find the corresponding hex code
```
echo -n "&" | hexdump -C
```

## How to redirect standard error in bash

[nixCraft Article](https://www.cyberciti.biz/faq/how-to-redirect-standard-error-in-bash/)

| Command | Description/Purpose |
|:--|:--|
| command 2>filename | Redirect stderr to filename |
| command >output 2>filename | Redirect stderr to file named filename and stdout to file named output |
| command &> filename | Redirect stderr and stdout to filename |
| command 2>&- | Just suppress error messages. No file created. No error message displayed on screen |
| command 2>/dev/null | Just suppress error messages. |
| command 2>&1 | Redirect error messages to standard output. Useful in shell script when you need to forcefully display error messages on screen |


## Removing blank spaces and special chars

**Note that we cannot just type the accent ^, instead we must use CTRL+V and then type CTRL+M.**

### Using Vim Editor
- If you want to remove blank lines (using vi):
`:g/^$/d`

- For commented lines (using vi):
`:g/^#/d`

- For lines where windows editor put ^M and you want to remove (using vi).
`:%s/^V^M//g`

- The final command will look like this:
`:%s/^M//g`

### Using sed
```
sed -e "s/^M//" file-orig.sh > file-mod.sh
```

### Using Dos2Unix
```
dos2unix input
dos2unix -b input
```

### Using tr
To delete a CRLF:
```
tr -d '\r' < input > output
```

### Using egrep
````
egrep -v "^#|^$" squid.original > squid.conf
````

### Using grep
```
grep -v ^["#"] squid.original > squid.conf
```

---

## Interactive shell

### Using rlwrap
```
rlwrap nc -nlv 443
rlwrap ./exploit.sh
```

### Using python
- Once you have a shell on the victim:
```
python -c 'import pty;pty.spawn("/bin/bash")'
```
Or
```
/usr/bin/python3 -c 'import pty;pty.spawn("/bin/bash")'
```

- Press CTRL+Z
- On attacker's machine:
```
stty raw -echo
```

- Now press `fg` to get the shell back

---

## Backup of files permissions

- To create the permission file
```
getfacl -Rp /home/psylinux/Desktop > /home/psylinux/permissoes_desktop.txt
```

- To restore the permissions from file
```
setfacl --restore=/home/psylinux/permissoes_desktop.txt -R
```

---

## Smart management of soft links with _stow_
- Installation

```
apt install stow
```

- Managing soft links
```
stow -v -t Target Folder1 Folder2 Folder3
```

```
stow -v -D -t Target Folder1 Folder2 Folder3
```

```
stow -v -R -t Target Folder1 Folder2 Folder3 Folder4
```

```
stow -v -R -t Target Folder1 --ignore='SubFolder1' --ignore='SubFolder2'
```

```
stow -v -R -t Target Folder1 --ignore='(?:\..*|[^+]*\+[^+]*)'
```
