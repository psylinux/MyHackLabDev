---
author: 'Marcos Azevedo'
date: '2021-03-25'
linktitle: 'Cheaty Sheet NEW'
title: 'Cheaty Sheet NEW'
weight: 10

---


## New Thing goes here

### Start a New Session

- This here

  ```bash
  tmux new-session
  ```


- Or you can type

  ```bash
  tmux
  ```

- Or you can use the shorthand

  ```bash
  tmux new
  ```


- If you're already inside the tmux

  ```bash
  : new
  ```

### Start a New Session with the name mysession

```bash
tmux new -s mysession : new -s mysession
```

### Kill-Delete Sessions

- Kill-Delete mysession

```bash
tmux kill-session -t mysession
```

- Or you can use the shorthand

```bash
tmux kill-ses -t mysession
```

- Kill/Delete all sessions but mysession

```bash
tmux kill-session -a -t mysession
```

- Kill/Delete all sessions but the current session

```bash
tmux kill-session -a
```