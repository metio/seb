---
title: Encrypt dotfiles with chezmoi
date: 2021-09-20
categories:
- snippet
tags:
- dotfiles
- chezmoi
- gpg
---

[chezmoi](https://www.chezmoi.io/) can use various external tools to [keep data private](https://www.chezmoi.io/docs/how-to/#keep-data-private). I prefer to use [gpg](https://www.gnupg.org/) since that is already installed/configured on my system anyway. In order to configure it, just set yourself as the recipient like this:

```toml
[gpg]
  recipient = "seb@hoß.de"

```

Calling `chezmoi add --encrypt /path/to/secret` will now create encrypt the file with your public key which allows you to decrypt them later with your private key.
