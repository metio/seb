---
title: Automatically update plugins for vim/nvim
date: 2022-01-01
menu: topic
categories:
- snippet
tags:
- vim
- neovim
- plugins
- updates
- systemd
- shell
- git
---

Both [Vim](https://www.vim.org/) and [Neovim](https://neovim.io/) have a built-in plugin mechanism that loads plugins from `~/.vim/pack/*/start/{name}` (Vim) or `~/.local/share/nvim/site/pack/*/start` (Neovim). All you have to do in order to install new plugins, is to `git clone` their repository into those directories. In order to automatically update those clones, I'm using the following script:

```shell
#!/usr/bin/env zsh

###############################################################################
# This script git-pulls all installed nvim plugins which are using the built-in
# nvim plugin manager. Those plugins are located in .local/share/nvim/site/pack
#
# Required software that is not in GNU coreutils:
#   - 'git' to fetch plugin updates from upstream
###############################################################################

### User specific variables, adjust to your own needs

# folder that contains all nvim plugins
PLUGIN_DIR="${XDG_DATA_HOME:-$HOME/.local/share}/nvim/site/pack"

echo "updating all plugins in ${PLUGIN_DIR}"
# iterate through all directories and git pull em
for directory in "${PLUGIN_DIR}"/*/{start,opt}/*; do
    if [ -d "${directory}" ]; then
        plugin=$(basename "${directory}")
        echo "updating ${plugin}"
        git -C "${directory}" pull --quiet
    fi
done
```

In case you are using Vim, adjust the `PLUGIN_DIR` variable to point to your Vim plugin directory instead. Since all good developers must be lazy, I've added the following [systemd service](https://www.freedesktop.org/software/systemd/man/systemd.service.html) to execute the above script automatically:

```unit file (systemd)
[Unit]
Description=cron job that triggers an update of all nvim plugins
Wants=network-online.target
After=network-online.target

[Service]
Type=oneshot
ExecStart=/var/home/seb/.local/bin/update-nvim-plugins.sh
RemainAfterExit=false
```

Adjust the `ExecStart` line to match the location where you saved the above script and place that service definition in a file called `nvim-plugins-update.service` into your local `~/.config/systemd/user` directory. Add another file called `nvim-plugins-update.timer` next to it that defines a [systemd timer](https://www.freedesktop.org/software/systemd/man/systemd.timer.html) with the following content:

```unit file (systemd)
[Unit]
Description=Update nvim plugins every week

[Timer]
OnCalendar=weekly
Persistent=true
RandomizedDelaySec=5hours

[Install]
WantedBy=timers.target
```

Adjust how often you want to update the plugins you are using in the `OnCalendar` line. Enable this service/timer with:

```shell
$ systemctl --user enable nvim-plugins-update
```

Finally, I've added the following shell aliases to make it easier to interact with the created systemd units:

```shell
# trigger an update manually
alias update-nvim-plugins='systemctl --user start nvim-plugins-update'
# see status of last auto-update
alias update-nvim-plugins-status='systemctl --user status nvim-plugins-update'
# see logs of past auto-updates
alias update-nvim-plugins-logs='journalctl --user --unit nvim-plugins-update'
```
