+++
title = "Debugging with strace"
description = "Debugging with strace"
tags = [
    "linux",
    "strace",
    "debug",
    "development",
]
date = "2019-01-13"
categories = [
    "Development",
    "Linux",
    "Strace",
    "Debug"
]
+++

### Preface
Strace is a debugging tool that help us to troubleshoot issues. Strace monitors the system calls and signals of a specific program and it is helpful when we do not have the source code and would like to debug the execution of a program. The strace provides us the execution sequence of a binary from start to end including a very nice matrix that we'll look at this article.


Strace can show us all passed arguments with great filter capabilities.

* Tracing an execution
    -o write to an output file
    -t shows the timestamp
    -r shows the relative timestamp
```
strace Binary_File
```

* Using filters
We can call the program with arguments and use the -e option to filter just what we want:
```
strace -e socket, send, recv  nc google.com 80
```

* Attaching to a running process
```
strace -p PID
```

* Show the program statistics  
```
strace -c binary_file arguments
```

