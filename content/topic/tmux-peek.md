---
title: Peeking with tmux
date: 2021-04-05
menu: topic
categories:
- snippet
tags:
- tmux
- peek
---

[tmux](https://github.com/tmux/tmux) uses can use the following snippet to peek at files. Place it in your `.bashrc` or similar file.

```shell script
peek() { tmux split-window -p 33 "$EDITOR" "$@" }
```

Calling `peek <file>` will open `<file>` in lower third of tmux window.
