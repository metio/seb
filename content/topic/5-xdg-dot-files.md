---
title: XDG Base Directory Specification
date: 2020-07-27
menu: topic
categories:
- snippet
tags:
- dot files
- xdg
---

The [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) has been around for a while, yet not every software has adopted it yet. Here is an incomplete list of fixes:

```shell script
# use existing env variables or define new
[ -z "$XDG_CACHE_HOME"  ] && export XDG_CACHE_HOME="$HOME/.cache"
[ -z "$XDG_CONFIG_DIRS" ] && export XDG_CONFIG_DIRS="/etc/xdg"
[ -z "$XDG_CONFIG_HOME" ] && export XDG_CONFIG_HOME="$HOME/.config"
[ -z "$XDG_DATA_DIRS"   ] && export XDG_DATA_DIRS="/usr/local/share:/usr/share"
[ -z "$XDG_DATA_HOME"   ] && export XDG_DATA_HOME="$HOME/.local/share"

# gradle
export GRADLE_USER_HOME="$XDG_DATA_HOME/gradle"

# httpie
export HTTPIE_CONFIG_DIR="$XDG_CONFIG_HOME/httpie"

# npm
export NPM_CONFIG_USERCONFIG="$XDG_CONFIG_HOME/npm/npmrc"
export npm_config_cache="$XDG_CACHE_HOME/npm"

# password-store
export PASSWORD_STORE_DIR="$XDG_DATA_HOME/password-store"
```

In order to make your own software XDG-aware, consider using the [dirs-dev](https://github.com/dirs-dev) libraries.
