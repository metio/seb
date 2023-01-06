---
title: Maintaining dotfiles with chezmoi
date: 2021-10-11
categories:
- snippet
tags:
- dotfiles
- chezmoi
---

To make it easier managing many dotfiles with [chezmoi](https://www.chezmoi.io/), a shell function similar to the one below can be used:

```shell script
function m-dotfiles-ok {
    # public
    chezmoi add ~/.config/zsh --recursive
    chezmoi add ~/.config/sway --recursive
    chezmoi add ~/.config/tmux --recursive
    chezmoi add ....

    # secrets
    chezmoi add --encrypt ~/.config/npm/npmrc
    chezmoi add --encrypt ~/.ssh/id_rsa
    chezmoi add --encrypt ...
}
```

Whenever you feel happy with your current setup, just call `m-dotfiles-ok` to push changes into the chezmoi source directory. Files will automatically be [encrypted](../chezmoi-gpg) with [gpg](https://www.gnupg.org/) and [committed/pushed](../chezmoi-auto-git) into a [Git](https://git-scm.com/) repository if you have done the necessary configuration beforehand.

In general, editing your dotfiles directly as explained in the second option of the [FAQ](https://www.chezmoi.io/user-guide/frequently-asked-questions/usage/#how-do-i-edit-my-dotfiles-with-chezmoi) seems easier though. Refactoring your dotfiles is especially easy when the `exact_` prefix is used for directories. As explained in the [documentation](https://www.chezmoi.io/reference/target-types/#directories), all files that are not managed by `chezmoi` will be removed, therefore your configuration will always match what is in your source directory.
