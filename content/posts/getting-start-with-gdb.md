+++
title = "Getting Started with GDB"
description = "Using the GNU Linux Debugger"
tags = [
    "linux",
    "gdb",
    "gef",
    "development",
]
date = "2018-12-16"
categories = [
    "Development",
    "Linux",
    "GDB",
    "C Lang"
]
+++

## Step 1. Configuring Plugins

+++ Using [GEF]

+++ Compiling with debug symbols
gcc -ggdb source.c -o progma_with_symbols

+++ Stripping symbols off a binary
--strip-debug --strip-unneeded  binary_to_strip

+++ Ripping Symbols off a binary
objcopy --only-keep-debug rip_from_binary debug_file

+++ Switch beetween AT&T and intel Syntax
set disassembly-flavor intel

+++ To run the program
run args

+++ Listing the source file (Just when source file is available at the same folder and file name)
list    (Will start looking near at the main function)
list 1  (Will list from the first line)

+++ To get info about registers
info registers

+++ To list all functions of the program
info functions

+++ To list all sources where symbols were read
info sources

+++ To get info about the program
info souce

+++ To list global variables and static
info variables

+++ To get local variables
info scope Function_Name

+++ Working with breakpoints
    # Set a breakpoint
    break position

    # List breakpoints
    info breakpoints

    # Enable/Disable breakpoints
    disable 1
    enable 1

    # Deleting breakpoints
    delete 1

+++ Modifying Memory and Registers
set {char} 0xbffff7e6 = 'B'
set {int} (0xbffff7e6 + 1) = 66
set var1 = 100
set $eax = 10

+++ Defining macros
define hook-stop
    command 1
    command 2
    command 3
end

+++ Working with Data
++++ Checking ‘REGISTERS’ and ‘MEMORY’
Display Register Values : (Decimal , Binary , Hex )
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
(gdb)

++++ Display values of specific memory locations :
command : x/nyz (Examine)
n –> Number of fields to display ==>
y –> Format for output ==> c (character) , d (decimal) , x (Hexadecimal)
z –> Size of field to be displayed ==> b (byte) , h (halfword), w (word 32 Bit)
