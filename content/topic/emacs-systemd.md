---
title: Emacs and systemd
draft: true
menu: topic
categories:
- snippet
tags:
- emacs
- systemd
---

```shell script
alias e='emacsclient --tty --socket-name=user'
alias vim='emacsclient --tty --socket-name=user'
alias vi='emacsclient --tty --socket-name=user'
alias nano='emacsclient --tty --socket-name=user'
alias ed='emacsclient --tty --socket-name=user'
```

Unit file `emacs@.service`:

```ini
[Unit]
Description=Emacs text editor [%I]
Documentation=info:emacs man:emacs(1) https://gnu.org/software/emacs/

[Service]
Type=forking
ExecStart=/usr/bin/emacs --daemon=%i
ExecStop=/usr/bin/emacsclient --eval "(kill-emacs)"
Environment=SSH_AUTH_SOCK=%t/keyring/ssh
Restart=on-failure

[Install]
WantedBy=default.target
```
