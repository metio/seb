---
title: Using chezmoi with age
date: 2023-01-08
categories:
- snippet
tags:
- dotfiles
- chezmoi
- age
- encryption
---

[age](https://age-encryption.org/) is another tool supported by [chezmoi](https://www.chezmoi.io/) to [keep data private](https://www.chezmoi.io/docs/how-to/#keep-data-private). Compared to `gpg` it is much simpler by focusing on the encryption parts only.

Add the following snippet to your `.chezmoi.toml` to configure `chezmoi` to use `age`:

```toml
encryption = "age"
[age]
  identity = "path/to/age/private-key"
  recipient = "age...public...key..."
```

Adding files to your `chezmoi` source directory remains the same as compared to using `gpg` - just call `chezmoi add --encrypt path/to/file`.
