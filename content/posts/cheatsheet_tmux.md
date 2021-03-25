---
author: "Marcos Azevedo"
date: 2021-03-25
linktitle: "Cheat Sheet :: Tmux"
title: "Cheat Sheet :: Tmux"
categories: [ "cheatsheet", "sysadmin", "shell" ]
tags: [ "cheatsheet", "shell", "tmux", "linux", "sysadmin" ]
weight: 10
---


# TMUX

## Working with Sessions

### Start a New Session
```
$ tmux new-session
```

- Or you can type
```
$ tmux
```

- **Or you can use the shorthand**
```
tmux new
```

- **If you're already inside the `tmux`**
```vim
: new
```


### Start a New Session with the name **mysession**
```
$ tmux new -s mysession : new -s mysession
````


### Kill/Delete Sessions

- Kill/Delete **mysession**
```
$ tmux kill-session -t mysession
````

- **Or you can use the shorthand**
```
$ tmux kill-ses -t mysession
````

- Kill/Delete all sessions *but* **mysession**
```
$ tmux kill-session -a -t mysession
```

- Kill/Delete all sessions *but* the **current** session
```
$ tmux kill-session -a
```


### Rename session

- **If you're already inside the `tmux`**
```vim
Ctrl + b + $
```

### Detach from session
- **If you're already inside the `tmux`**
```vim
Ctrl + b + d
```


### Move Between Sessions
- Move to previous session // **If you're already inside the `tmux`**
```vim
Ctrl + b + (
```

- Move to next session // **If you're already inside the `tmux`**
```vim
Ctrl + b + )
```


### Attach to the last session
```
$ tmux attach-session
```

- Or
```
$ tmux attach
```

- Or
```
$ tmux at
```

- **Or you can use the shorthand**
```
$ tmux a
```


### Attach to a session using a name

- Attach to a session with the name **mysession**
```
$ tmux attach-session -t mysession
```

- Or
```
$ tmux attach -t mysession
```

- Or
```
$ tmux at -t mysession
```

- **Or you can use the shorthand**
```
$ tmux a -t mysession
```


### Show all sessions

- Listing all sessions
```
$ tmux list-sessions
```

- **Or you can use the shorthand**
```
$ tmux ls
```

- **If you're already inside the `tmux`**
```vim
Ctrl + b + s
```


## Working with Windows

### Start a new Window
- Start a new session with the name **mysession** and window name **mywindow**
```
$ tmux new -s mysession -n mywindow
```

- Create a new Window // **If you're already inside the `tmux`**
```vim
Ctrl + b + c
```


### Rename a Window
- Rename current window
```vim
Ctrl + b + ,
```

- Close the current window
```vim
Ctrl + b + &
```

### Moving Between Windows
- Previous window
```vim
Ctrl + b + p
```

- Next window
```vim
Ctrl + b + n
```

- Select window by number
```vim
Ctrl + b + 0...9
```

### Reorder a Window

- Swap window number 2 and 1
```vim
: swap-window -s 2 -t 1
```

## Working with Panes

### Toggle Panes
- Toggle last active pane
```vim
Ctrl + b + ;
```


### Split the Pane
- Split pane vertically
```vim
Ctrl + b + %
```

- Split pane horizontally
```vim
Ctrl + b + "
```


### Moving Between Panes
- Move the current pane to the left
```vim
Ctrl + b + {
```

- Move the current pane t the right
```vim
Ctrl + b + }
```


### Switching Between Panes
- Switch to pane to the direction
```vim
Ctrl + b + ⬆
Ctrl + b + ⬇
Ctrl + b + ⬅
Ctrl + b + ⮕
```


### Toggle synchronize-panes
- Send the same command to all panes
```vim
: setw synchronize-panes
```


### Toggle Between Panes
- Toggle between pane layouts
```vim
Ctrl + b + <SpaceBar>
```

- Switch to next pane
```vim
Ctrl + b + o
```

- Show pane numbers (Type the number to go to that pane)
```vim
Ctrl + b + q
```


### Toggle the Pane Zoom
```vim
Ctrl + b + z
```


### Convert a Pane into a Window
```vim
Ctrl + b + !
```


### Resize the Pane
- Resize the current pane height
```vim
Ctrl + b + ⬆
Ctrl + b + ⬇
```

- Resize the current pane width
```vim
Ctrl + b + ⬅
Ctrl + b + ⮕
```

### Close the Pane
- Close the Current Pane
```vim
Ctrl + b + x
```


## Enter in Copy Mode (Buffer)

- Enter in Copy Mode
```vim
Ctrl + b + [
```

- Enter in Copy Mode and Scroll One Page Up
```vim
Ctrl + b + PgUp
```

- Quit Mode
```vim
q
```

- Go to Top Line
```vim
g
```

- Go to Bottom Line
```vim
G
```

### Scroll using the Keyboard Arrows
- Scroll Up
```vim
⬆
```

- Scroll Down
```vim
⬇
```

- Scroll Left
```vim
⬅
```

- Scroll Right
```vim
⮕
```

### Scroll using the `vim` keys

**FIRST: Remember to Set the `vim` keys in Buffer Mode***. Go to [[cheatsheet_tmux#Set the runtime tmux Options]] to learn more.

- Move Cursor to the Left
```vim
h
```

- Move Cursor Down
```vim
j
```

- Move Cursor to the Up
```vim
k
```

- Move Cursor to the Right
```vim
l
```

- Move Cursor Forward One Word At a Time
```vim
w
```

- Move Cursor Backward One Word At a Time
```vim
b
```

- Search forward
```vim
/
```

- Search backward
```vim
?
```

- Go to the Next Keyword Occurrence
```vim
n
```

- Go to the Previous Keyword Occurrence
```vim
N
```


### Working with Buffers
- Start a Selection
```vim
<SpaceBar>
```

- Clear the Selection
```vim
<Esc>
```

- Copy selection
```vim
<Enter>
```

- Paste the Contents of Buffer_0
```vim
Ctrl + b + ]
```

- Display the Buffer_0 Contents
```vim
: show-buffer
```

- Copy the Entire Visible Contents of Pane to a Buffer
```vim
: capture-pane
```

- Show All Buffers
```vim
: list-buffers
```

- Show All Buffers and Paste Selected
```vim
: choose-buffer
```

- Save the Buffer Contents to the file `buf.txt`
```vim
: save-buffer buf.txt
```

- Delete the Buffer_1
```vim
: delete-buffer -b 1
```

- Enter the Command Mode
```vim
Ctrl + b + :
```

## Set the runtime `tmux` Options
- Set OPTION for All Sessions
```vim
: set -g OPTION
```

- Set OPTION for All Windows
```vim
: setw -g OPTION
```

- Set to use `vim` keys in Buffer Mode
```vim
: setw -g mode-keys vi
```

- Set the `vim` Buffer Size in Runtime (history)
```vim
set-option -g history-limit 50000
```

- Start a New Session with a Defined Buffer Size (history)
```
tmux set-option -g history-limit 50000; new-session
```


## Help
- Show Every Session, Window, Pane, etc...
```
$ tmux info
```

- Show Shortcuts
```vim
Ctrl + b + ?
```
