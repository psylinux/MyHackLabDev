---
author: Marcos Azevedo
date: '2018-12-16T17:38:15-03:00'
linktitle: 'Article :: Getting Started with GDB'
title: 'Article :: Getting Started with GDB'
categories:
  - article
  - linux
  - debug
  - development
tags:
  - article
  - linux
  - gdb
  - gef
  - development
weight: 10
date updated: '2021-03-25T17:35:37-03:00'

---


## Preface
1. Before start, take a look at [GCC Compiling Cheat Sheet](/post/gcc-cheat-sheet).
2. Go to [Installing GEF (GDB Plug-in)](/posts/installing-gdb-gef), to use GEF Plug-in.


## Compiling with debug symbols
```bash
gcc -ggdb source.c -o prog_with_symbols
```

## Stripping symbols off the binary
1. Using strip command to rip off all symbols from a binary file
```bash
strip --strip-debug --strip-unneeded prog_not_stripped -o prog_nodebug_stripped
```

2. Using objcopy command to create a separated debug file
```bash
objcopy --only-keep-debug rip_from_binary debug_file
```

## Adding Debug Symbols to a binary
1. Add it in the binary itself
```bash
objcopy --add-gnu-debuglink=debug_file binary_file
```

2. Load the symbol file within GDB
```bash
symbol-file debug_file
```

----
## Analyzing Symbols with NM
* Lower case in local symbols
* Upper case in external symbols

For more information about symbol types use `man nm`

| Symbol Type | Meaning |
|:--|:--|
| A | Absolute symbol                         |
| B | In the Uninitialized Data Section (BSS) |
| D | In the Initialized Data Section         |
| N | Debugging Symbol                        |
| T | In the Text Section                     |
| U | Symbol Undefined Right Now              |



### Some useful command line option for nm

#### Using nm to search for a symbol and display the file name:
```bash
nm -A Binary_File | grep function_name
```

#### Display symbols ordered by address:
```bash
nm -n Binary_File
```

#### Display all the external symbols:
```bash
nm -g Binary_File
```

#### Display all symbols, even debugger-only symbols; normally these are not listed:
```bash
nm -a Binary_File
```

#### Listing only symbols only in the TEXT section
```bash
nm -a Binary_File | grep ' T ' # must leave the spaces around the T
```

----

## Decompiling using objdump
```bash
objdump -M intel -D a.out | grep -A20 main.:
```

----

## GDB Cheat Sheet

### Switching between AT&T and Intel Syntax
```bash
set disassembly-flavor intel
```

### Disassembling
```bash
disassemble /r main (We can use /r to show the opcodes)
disassemble main
disassemble _start
disassemble 0x80484b0
```

Two arguments (separated by a comma) are taken as a range of memory to dump, in the form of "start,end", or "start,+length".
```bash
disassemble main,+30
```

### To run the program
```bash
run args
```

### Listing the source file
**Just work when source file is available at the same folder and with the same file name**
```bash
list    (Will start looking near at the main function)
list 1  (Will list from the first line)
```

### To get info about registers
```bash
info registers
```

### To list all functions of the program
```bash
info functions
```

### To list all sources where symbols were read
```bash
info sources
```

### To get info about the program
```bash
info source
```

### To list global variables and static (not local variables)
```bash
info variables
```

### To list local variables
```bash
info scope Function_Name
```

### To list all symbols
```bash
maintenance print symbols
maintenance print symbols file_to_store
```

### Working with breakpoints

#### Set a breakpoint
```bash
break position
```

#### List breakpoints
```bash
info breakpoints
```

#### Enable/Disable breakpoints
```bash
disable 1
enable 1
```

#### Deleting breakpoints
```bash
delete 1
```

### Modifying Memory and Registers
```bash
set {char} 0xbffff7e6 = 'B'
set {char} 0x080484b0 = 0x00000001b8 (opcode of "mov eax,0x1")
set {int} (0xbffff7e6 + 1) = 66
set var1 = 100
set $eax = 10
```

### Defining macros
```bash
define hook-stop
    command 1
    command 2
    command 3
end
```

### Working with Data

#### Checking _REGISTERS_ and _MEMORY_

**Display Register Values : (Decimal, Binary, Hex)**
```bash
print /d –> Decimal
print /t –> Binary
print /x –> Hex
O/P :
(gdb) print /d $eax
$17 = 13
(gdb) print /t $eax
$18 = 1101
(gdb) print /x $eax
$19 = 0xd
```

#### Display values of specific memory locations

**command "Examine": x/nyz**

* n –> Number of fields to display ==>
* y –> Format for output ==> c (character) , d (decimal) , x (Hexadecimal)
* z –> Size of field to be displayed ==> b (byte), h (halfword), w (word)

#### Convenience variables
```bash
(gdb) set $i = 10
(gdb) set $dyn = (char \*)malloc(10)
(gdb) $demo = "psylinux"
(gdb) set argv[1] = $demo
```

#### Calling functions
```bash
(gdb) info functions
(gdb) call Function_1(args_list)
(gdb) call strlen("psylinux")
(gdb) call strcpy ($dyn, argv[1])
```

### Conditional breakpoint
Break only if the condition is satisfied.
```bash
(gdb) break *0x0804844b
(gdb) condition 1 $eax == 0
(gdb) info b
Num     Type           Disp Enb Address    What
1       breakpoint     keep y   0x0804844b in main at main.c:8
        stop only if $eax == 0
        breakpoint already hit 1 time
```
