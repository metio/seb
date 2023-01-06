---
title: passage fuzzy search
date: 2022-12-27
categories:
- snippet
tags:
- passage
- age
- fuzzy
- search
- clipboard
---

In order to fuzzy search through passwords managed with [passage](https://github.com/FiloSottile/passage), I've written the following script that is inspired by the upstream version which is using `fzf`.

```shell
fd --type=file --base-directory="${PASSAGE_DIR:-${HOME}/.passage/store}" .age --exec echo '{.}' | \
  sk --cycle --layout=reverse --tiebreak=score --no-multi | \
  xargs --replace --max-args=1 --no-run-if-empty \
    passage show --clip=1 {}
```

This version requires [fd](https://github.com/sharkdp/fd/), [skim](https://github.com/lotabout/skim), [xargs](https://www.gnu.org/software/findutils/manual/html_node/find_html/Invoking-xargs.html), and [passage](https://github.com/FiloSottile/passage) itself of course. The detailed breakdown on how it works is as follows:

1. Use `fd` to find all files within `${PASSAGE_DIR}` that end in `.age`. Each password in passage is inside that folder and has such a file extensions, thus we are selecting every password we have.
2. Using both `--base-directory` and `--exec echo '{.}'` ensures that passwords are returned in such form that they can be passed back into `passage` again. The placeholder `'{.}'` is a feature provided by `fd` which strips the file extension from each returned value.
3. All passwords are then passed into `sk` in order to allow to fuzzy search across them all. Setting `--no-multi` ensures that only a single password can be selected.
4. Finally, `xargs` calls `passage` and replaces the curly braces with the selected password. Thanks to `--clip=1`, the first line in the selected password entry will be copied to the clipboard and automatically cleared after 45 seconds.

In order to call that script, I've saved it as `passage-fuzzy-search.sh` in my `.local/bin` folder and added some checks into it to make sure that every required software is actually installed.

```shell
#!/usr/bin/env zsh

###############################################################################
# This shell script presents passwords saved with passage through skim
#
# Call it like this:
#   passage-fuzzy-search.sh
#
# Required software that isn't in GNU coreutils:
#   - 'passage' to read passwords
#   - 'fd' to find passwords
#   - 'sk' to filter passwords
###############################################################################

if ! (( ${+commands[passage]} )); then
    echo 'passage not installed. Please install passage.'
    exit
fi
if ! (( ${+commands[sk]} )); then
    echo 'sk not installed. Please install skim.'
    exit
fi
if ! (( ${+commands[fd]} )); then
    echo 'fd not installed. Please install fd-find.'
    exit
fi

fd --type=file --base-directory="${PASSAGE_DIR:-${HOME}/.passage/store}" .age --exec echo '{.}' | \
  sk --cycle --layout=reverse --tiebreak=score --no-multi | \
  xargs --replace --max-args=1 --no-run-if-empty \
    passage show --clip=1 {}
```

Since typing `passage-fuzzy-search.sh` is way too long, I have added an alias like this:

```shell
alias pp='passage-fuzzy-search.sh'
```
