---
title: Use tmux as your login shell
date: 2021-04-19
categories:
- snippet
tags:
- tmux
- login shell
- chsh
---

To use [tmux](https://github.com/tmux/tmux) as your login shell, use `chsh`:

```shell script
# list all available shells
$ chsh --list-shells
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin
/usr/bin/zsh
/bin/zsh
/usr/bin/tmux
/bin/tmux

# select login shell
$ chsh --shell /usr/bin/tmux
```


