---
author: Marcos Azevedo
date: '2019-01-13T17:38:15-03:00'
linktitle: 'Article :: Debugging with strace'
title: 'Article :: Debugging with strace'
categories:
  - article
  - linux
  - debug
  - development
tags:
  - article
  - linux
  - strace
  - debug
  - development
weight: 10
date updated: '2021-03-25T17:35:10-03:00'

---


# Strace debugging tool

## Preface
Strace is a debugging tool that help us to troubleshoot issues. Strace monitors the system calls and signals of a specific program and it is helpful when we do not have the source code and would like to debug the execution of a program. The strace provides us the execution sequence of a binary from start to end including a very nice matrix that we'll look at this article.
Strace can show us all passed arguments with great filter capabilities.

## Tracing an execution
    -x write to an output file
    -t shows the timestamp
    -r shows the relative timestamp
```bash
$ strace Binary_File
```

## Using filters
We can call the program with arguments and use the -e option to filter just what we want:
```bash
$ strace -e socket, send, recv nc google.com 80
```

## Attaching to a running process
```bash
$ strace -p PID
```

## Show the program statistics
```bash
$ strace -c binary_file arguments
```
