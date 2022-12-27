---
title: chezmoi & shell init scripts
date: 2022-12-12
menu: topic
categories:
- snippet
tags:
- chezmoi
- dotfiles
- shell
- performance
---

Many CLI applications offer initialization scripts to integrate into a shell, e.g. `starship init zsh` or `zoxide init zsh`. The documentation of these tools usually tell you to put something like `eval "$(starship init zsh)"` into your shell RC file. While this approach works fine, it does decrease startup speed of your shell because it needs to run the `init` command every time you open a new shell. Given that you open shells much more often than new versions of these tools are released/installed, I wanted to cache the output of these commands in order to get a bit of speed back.

[chezmoi](https://chezmoi.io/) provides a template function called [output](https://www.chezmoi.io/reference/templates/functions/output/) which replaces itself with the output of the command you specified. In order to integrate various tools into my shell, I've used that function like this (using zsh):

1. Create a directory that holds all init scripts for every tool you want to use.
   ```shell
    $ mkdir --parents "${ZDOTDIR}"/tools.d
    ```
2. Let your shell load all available scripts in that directory:
    ```shell
    for init_script in "${ZDOTDIR}"/tools.d/*.sh; do
      source "${init_script}"
    done
   ```
3. Create chezmoi `.tmpl` files for each tool and place them in the chezmoi source directory that matches the directory you created in step 1:
    ```go-text-template
   {{ output "starship" "init" "zsh" "--print-full-init" }}
   ```
4. Call `chezmoi apply` in order to generate the init scripts.

The only downside here is that you have to re-run `chezmoi apply` after updating one of the tools because they change their init scripts sometimes. I solved that problem with [chezmoi auto-updates](../chezmoi-auto-update).
