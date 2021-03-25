---
author: Marcos Azevedo
date: '2019-01-14T17:38:15-03:00'
linktitle: Patching binary files in Linux
title: Patching binary files in Linux
categories:
  - howto
  - assembly
  - development
  - exploitation
tags:
  - howto
  - assembly
  - nasm
  - dev
  - glibc
  - linux
  - userland
  - exploitation
weight: 10
date updated: '2021-03-25T17:37:57-03:00'

---


# Patching binary files in Linux

## Using xxd

### Binary to Hex
```
$ xxd prog1.bin > b1.hex
$ xxd prog1.bin > b2.hex
```
### Hex to Binary
```
$ xxd -r -p b1.hex new_prog1.bin
```
* Then we can generate the diff like this:
```
diff b1.hex b2.hex
```
or
```
vimdiff b1.hex b2.hex
```

## Using bsdiff
```
$ apt-get install bsdiff

$ bsdiff old_prog.bin new_prog.bin patch.bin
$ bspatch old_prog.bin output.bin patch.bin
```