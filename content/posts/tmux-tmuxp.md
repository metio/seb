---
title: Manage tmux sessions with tmuxp
date: 2021-12-20
categories:
- snippet
tags:
- tmux
- tmuxp
---

To manage [tmux](https://github.com/tmux/tmux) sessions, I like to use [tmuxp](https://github.com/tmux-python/tmuxp). It works by having pre-defined sessions in `~/.config/tmuxp` which looks like this:

```yaml
session_name: cool-app
start_directory: ~/projects/cool-app
windows:
- window_name: backend
  start_directory: backend
- window_name: frontend
  start_directory: frontend
```

In case the name of the file is `cool-app.yaml`, you can open the sessions with `tmuxp load cool-app --yes`.
