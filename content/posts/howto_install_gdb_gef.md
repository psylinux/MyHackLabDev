---
author: Marcos Azevedo
date: '2018-12-16T17:38:15-03:00'
linktitle: 'HowTo :: Install GEF (GDB Plug-in)'
title: 'HowTo :: Install GEF (GDB Plug-in)'
categories:
  - howto
  - linux
  - debug
  - development
tags:
  - howto
  - linux
  - gdb
  - gef
  - development
weight: 10
date updated: '2021-03-25T17:37:17-03:00'

---


## Cloning
Goto [GEF Project](https://github.com/hugsy/gef) and clone the repository to __/opt__
```bash
cd /opt
git clone https://github.com/hugsy/gef.git
```

## Checking
### To check if the GDB was compiled with Python support run the following command in terminal:
```bash
gdb -nx -ex 'pi print(sys.version)' -ex quit
```

### The output should be similar to this:
```bash
2.7.3 (default, Mar 18 2014, 06:31:17)
[GCC 4.6.3]
```

## Installing
To install the gef plug-in, just run the shell scripts as following:
```bash
/opt/gef/scripts/gef.sh
/opt/gef/scripts/gef-extras.sh
```

## Configuring
* Use the *pyenv* to set the correct python version
* Enter the GDB and type: _gef missing_
* Use *pip* to install the GEF dependences


## Updating
To keep the GEF plug-in update, just do:
```bash
python ~/.gdbinit-gef.py --update
```
