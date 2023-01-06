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

Both [Vim](https://www.vim.org/) and [Neovim](https://neovim.io/) have a [built-in](https://vimhelp.org/repeat.txt.html#packages) [plugin mechanism](https://neovim.io/doc/user/usr_05.html#plugin) that loads plugins from `~/.vim/pack/*/{start,opt}/*` (Vim) or `~/.local/share/nvim/site/pack/*/{start,opt}/*` (Neovim). All you have to do in order to install new plugins, is to `git clone` their repository into those directories. In order to automatically update those clones, create the following script:

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

### Script logic

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

In case you are using Vim, adjust the `PLUGIN_DIR` variable to point to your Vim plugin directory instead and save the resulting shell script as a file called `update-nvim-plugins.sh` in some folder of your choice. Do not forget to set the executable bit with `chmod +x /path/to/your/folder/update-nvim-plugins.sh`. Since all good developers must be lazy, write the following [systemd service](https://www.freedesktop.org/software/systemd/man/systemd.service.html) to execute the above script automatically:

```service
[Unit]
Description=cron job that triggers an update of all nvim plugins
Wants=network-online.target
After=network-online.target

[Service]
Type=oneshot
ExecStart=/path/to/your/folder/update-nvim-plugins.sh
RemainAfterExit=false
```

Adjust the `ExecStart` line to match the location where you saved the above script and place that service definition in a file called `nvim-plugins-update.service` into your local `~/.config/systemd/user` directory. Add another file called `nvim-plugins-update.timer` next to it that defines a [systemd timer](https://www.freedesktop.org/software/systemd/man/systemd.timer.html) with the following content:

```timer
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

```console
$ systemctl --user enable nvim-plugins-update
```

Finally, add the following shell aliases to make it easier to interact with the created systemd units:

```shell
# trigger an update manually
alias update-nvim-plugins='systemctl --user start nvim-plugins-update'
# see status of last auto-update
alias update-nvim-plugins-status='systemctl --user status nvim-plugins-update'
# see logs of past auto-updates
alias update-nvim-plugins-logs='journalctl --user --unit nvim-plugins-update'
```

With this setup in place, all your plugins will be automatically updated once per week or however often you have configured in the timer.
