---
author: "Marcos Azevedo"
date: 2018-12-16
linktitle: "Cheat Sheet :: GCC Compiling Cheat Sheet"
title: "Cheat Sheet :: GCC Compiling Cheat Sheet"
categories: [ "cheatsheet", "cross-compiling", "development" ]
tags: [ "cheatsheet", "linux", "windows", "gcc", "compiling", "cross-compiling", "development" ]
weight: 10
---


# GCC and Compiling Cheat Sheet

## Static compiling
```
gcc -static cve_2016_0728.c -o cve_2016_0728 -lkeyutils -Wall
```

## Cross compiling a C code from Linux to Windows
### Install mingw-w64 cross-compiler:
```
apt-get install mingw-w64
```

### To compile a Windows PE Executable 32bit:
```
i686-w64-mingw32-gcc 271.c -o 271
x86_64-w64-mingw32-gcc-win32 271.c -o 271
```

### Run the binary file with **wine**:
```
wine file.exe
```

## Compiling with debug symbols
Debug Symbol File Types:
1. DWARF 2
2. COFF
3. XCOFF
4. STABS

### GCC use the -g option to define the type:
```
gcc -ggdb source.c -o prog_with_symbols
```

### Stripping symbols off the binary
```
objcopy --strip-debug --strip-unneeded prog_not_stripped prog_stripped
```

### Ripping Symbols off the binary
```
objcopy --only-keep-debug rip_from_binary debug_file
```

## Having fun with C Pointers
```C
#include <stdio.h>

int main(void){
    int var = 5;
    int *p;

    printf("Address of var is: %p\n", &var);
    printf("Address of p is: %p\n", &p);

    p = &var;
    printf("Content of p is: %p\n", p);
    printf("Content wich p is pointing is: %i\n", *p);
    printf("Content of var is: %i\n", var);
    printf("Address of var is: %p\n", &var);
    printf("Address of p is: %p\n", &p);

    printf("*********************\n");
    *p = 100;
    printf("%i\n", var);
    printf("%i\n", *p);
    printf("Content of p is: %p\n", p);
    printf("Address of var is: %p\n", &var);
    printf("Address of p is: %p\n", &p);

    printf("*********************\n");
    p = 300;
    printf("%i\n", var);
    printf("%i\n", p);
    printf("Content of p is: %p\n", p);
    printf("Address of var is: %p\n", &var);
    printf("Address of p is: %p\n", &p);

    printf("*********************\n");
    printf("Final content of var: %i\n", var);
    printf("Content of p is: %p\n", p);
    return 0;
}
```
