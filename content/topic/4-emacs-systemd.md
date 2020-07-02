---
title: emacs and systemd
date: 2021-07-26
menu: topic
categories:
- snippet
tags:
- emacs
- systemd
---

I like to use [emacs](https://www.gnu.org/software/emacs/) to edit files in a terminal. It tends to start a little slow, thus I've created a [systemd](https://www.freedesktop.org/wiki/Software/systemd/) unit to automatically start the emacs daemon and use aliases to connect to the running daemon. The unit looks like this:

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

Enable it with `systemctl --user enable emacs@user` and define any number of aliases to make connecting to the emacs daemon easier:

```shell script
alias e='emacsclient --tty --socket-name=user'
alias vim='emacsclient --tty --socket-name=user'
alias vi='emacsclient --tty --socket-name=user'
alias nano='emacsclient --tty --socket-name=user'
alias ed='emacsclient --tty --socket-name=user'
```
