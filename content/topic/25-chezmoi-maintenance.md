---
title: Maintaining dotfiles with chezmoi
date: 2021-10-11
menu: topic
categories:
- snippet
tags:
- dotfiles
- chezmoi
---

In order to make my life easier managing lots of dotfiles with [chezmoi](https://www.chezmoi.io/), I've defined the following shell function:

```shell script
function m-dotfiles-ok {
    # public
    chezmoi add ~/.zshenv
    chezmoi add ~/.config/tmuxp/ --recursive
    chezmoi add ~/.config/zsh --recursive
    chezmoi add ~/.config/mako --recursive
    chezmoi add ~/.config/sway --recursive
    chezmoi add ~/.config/tmux --recursive
    chezmoi add ~/.config/alacritty --recursive
    chezmoi add ~/.config/environment.d --recursive
    chezmoi add ~/.config/git --recursive
    chezmoi add ~/.config/systemd --recursive

    # secrets
    chezmoi add ~/.ssh/config
    chezmoi add --encrypt ~/.config/npm/npmrc
    chezmoi add --encrypt ~/.ssh/id_rsa
    chezmoi add ~/.ssh/id_rsa.pub
}
```

Whenever I feel happy with my current setup, I just call `m-dotfiles-ok` to push my changes into my local chezmoi repository. Files will automatically be [encrypted](../encrypt-dotfiles-with-chezmoi) with [gpg](https://www.gnupg.org/) and [committed/pushed](../manage-dotfiles-with-chezmoi-and-git) into a [git](https://git-scm.com/) repository.
