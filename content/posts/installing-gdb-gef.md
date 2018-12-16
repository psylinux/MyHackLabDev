+++
title = "Installing GEF (GDB Plugin)"
description = "GDB Enhanced Features for exploit devs & reversers"
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


### Installing GEF

1. Goto [GEF releases](https://github.com/hugsy/gef) and clone the repository and clone this to /opt:

```
    cd /opt
    git clone https://github.com/hugsy/gef.git
```


2. Check if the GDB was compiled with Python support:

    **run the following command in terminal:**
```
        gdb -nx -ex 'pi print(sys.version)' -ex quit
```

    **The output should be similar to this:**
```
        2.7.3 (default, Mar 18 2014, 06:31:17)
        [GCC 4.6.3]
```


3. Installing:

    To install the gef pluging, just run the shell scripts as following:

```
    /opt/gef/scripts/gef.sh
    /opt/gef/scripts/gef-extras.sh
```


4. Configuring:

    * Use the pyenv to set the correct python version
    * Use pip to install the GEF dependences (see inside gdb: gef missing)

    
5. Updating:
    
```
    python ~/.gdbinit-gef.py --update
```
