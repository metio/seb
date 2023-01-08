---
title: Encrypt dotfiles with chezmoi
date: 2021-09-20
categories:
- snippet
tags:
- dotfiles
- chezmoi
- gpg
- encryption
---

**RECOMMENDATION**: Use [age](../chezmoi-age) instead of `gpg`.

[chezmoi](https://www.chezmoi.io/) can use various external tools to [keep data private](https://www.chezmoi.io/docs/how-to/#keep-data-private). [gpg](https://www.gnupg.org/) is used by various other tools as well, so chances are that you already have a functional setup on your system. To configure `gpg` with `chezmoi`, just set yourself as the recipient like this:

```toml
[gpg]
  recipient = "your.name@example.com"
```

Calling `chezmoi add --encrypt /path/to/secret` will now create encrypt the file with your public key which allows you to decrypt them later with your private key.
