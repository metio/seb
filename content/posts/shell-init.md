---
title: chezmoi & shell init scripts
date: 2022-12-12
categories:
- snippet
tags:
- chezmoi
- dotfiles
- shell
- performance
---

Many CLI applications offer initialization scripts to integrate into a shell, for example `starship init zsh` or `zoxide init zsh`. The documentation of these tools usually tell you to put something like `eval "$(starship init zsh)"` into your shell RC file. While this approach works fine, it does decrease startup speed of your shell because it needs to run the `init` command every time you open a new shell. Given that you open shells much more often than new versions of these tools are released and installed, you can cache the output of these commands to get a bit of speed back.

[chezmoi](https://chezmoi.io/) provides a template function called [output](https://www.chezmoi.io/reference/templates/functions/output/) which replaces itself with the output of the command you specified. You can use that function this to integrate various tools into your shell as the following example shows while using `zsh`:

1. Create a directory that holds all init scripts for every tool you want to use.
   ```shell
    $ mkdir --parents "${ZDOTDIR}"/tools.d
    ```
2. Let your shell load all available scripts in that directory. This snippet should be part of your `.zshrc` file:
    ```shell
    for init_script in "${ZDOTDIR}"/tools.d/*.sh; do
      source "${init_script}"
    done
   ```
3. Create chezmoi `.tmpl` files for each tool and place them in the chezmoi source directory that matches the directory you created in step 1:
    ```go-text-template
   {{ output "starship" "init" "zsh" "--print-full-init" }}
   ```
4. Call `chezmoi apply` to generate the init scripts.

The only downside here is that you have to re-run `chezmoi apply` after updating one of the tools because they change their init scripts sometimes. That problem can be solved with [chezmoi auto-updates](../chezmoi-auto-update).
