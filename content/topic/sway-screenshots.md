---
title: Taken screenshots on SwayWM
date: 2021-03-22
menu: topic
categories:
- snippet
tags:
- swaywm
- screenshot
- slurp
- grim
---

[SwayWM](https://swaywm.org/) uses can use a mixture of [grim](https://github.com/emersion/grim) and [slurp](https://github.com/emersion/slurp) to take screenshots of their desktop. Place the following key binding in your Sway config:

```shell script
# take screenshot of currently focused screen
bindsym $mod+Print exec /usr/bin/grim -o $(swaymsg -t get_outputs | jq -r '.[] | select(.focused) | .name') $(xdg-user-dir PICTURES)/$(date +'%Y-%m-%d-%H%M%S.png')

# take screenshot of selection
bindsym $mod+Shift+p exec /usr/bin/grim -g "$(/usr/bin/slurp)" $(xdg-user-dir PICTURES)/$(date +'%Y-%m-%d-%H%M%S.png')
```
