---
title: chezmoi auto-update
date: 2022-12-26
categories:
- snippet
tags:
- chezmoi
- dotfiles
- automation
---

In order to automatically synchronize dotfiles across my computers, I've written the following `systemd` unit:

```systemd
[Unit]
Description=Update chezmoi managed dotfiles
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/bin/chezmoi git -- pull --rebase
ExecStart=/usr/bin/chezmoi apply --no-tty --force
RemainAfterExit=false

[Install]
WantedBy=default.target
```

This unit pulls changes from upstream first and then applies the changes to the current computer after I'm logged in and a network connection is available. The `--no-tty` flag is required because there is no tty when systemd executes `chezmoi`. Likewise, the `--force` flag ensures that no interactive prompt will be displayed which we cannot answer since `systemd` is executing this unit without us being involved.
