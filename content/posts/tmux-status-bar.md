---
title: tmux Status Bar
date: 2021-08-09
categories:
- snippet
tags:
- tmux
- status bar
---


To see the currently active [tmux](https://github.com/tmux/tmux) status bar configuration, call:

```shell script
$ tmux show-options -g | grep status
```

Change on of those values with in the current tmux session:

```shell
$ tmux set-option status-right ""
```

Persist the change in your `tmux.conf` like this:

```shell script
# disable right side of status bar
set-option -g status-right ""
```
