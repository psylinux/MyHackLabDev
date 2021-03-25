---
author: Marcos Azevedo
date: '2019-01-04T17:38:15-03:00'
linktitle: 'HowTo :: Install PyEnv'
title: 'HowTo :: Install PyEnv'
categories:
  - howto
  - Development
  - Linux
  - Python
  - PyEnv
tags:
  - howto
  - linux
  - python
  - pyenv
  - development
weight: 10
date updated: '2021-03-25T17:37:34-03:00'

---


## Installing Dependencies
```
sudo apt install -y make build-essential libssl-dev \
zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl
```

## Installing PyEnv
```
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
```
