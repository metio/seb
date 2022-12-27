---
title: chezmoi auto-update
date: 2022-12-26
menu: topic
categories:
- snippet
tags:
- chezmoi
- dotfiles
- automation
---

In order to automatically synchronize dotfiles across my computers, I've written the following systemd unit:

```systemd
[Unit]
Description=Update chezmoi managed dotfiles
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/bin/chezmoi git -- pull --rebase
ExecStart=/usr/bin/chezmoi apply
RemainAfterExit=false

[Install]
WantedBy=default.target
```

This unit pulls changes from upstream first and then applies the changes to the current computer after I'm logged in and a network connection is available.
