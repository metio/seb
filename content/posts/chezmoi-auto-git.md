---
title: Manage dotfiles with chezmoi and git
date: 2021-09-06
menu: topic
categories:
- snippet
tags:
- dotfiles
- chezmoi
- git
---

[chezmoi](https://www.chezmoi.io/) can automatically commit and push changes to your [dotfiles](https://en.wikipedia.org/wiki/dotfile) into a (remote) [git](https://git-scm.com/) repository. Enable it with the following snippet in your `chezmoi.toml`

```toml
[sourceVCS]
    autoCommit = true
    autoPush = true
```

Every time you call `chezmoi add /path/to/file` will now create a new commit in your local chezmoi repository and push those changes into your configured remote repository.
