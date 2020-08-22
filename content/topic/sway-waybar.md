---
title: Waybar on SwayWM
date: 2021-08-23
menu: topic
categories:
- snippet
tags:
- swaywm
- waybar
---

[Waybar](https://github.com/Alexays/Waybar/) can be used as a status bar for [SwayWM](https://swaywm.org/). In order to use it, you tell Sway to use it with (e.g. `.config/sway/config`):

```
bar {
    swaybar_command waybar
}
```

Configure waybar itself in `~/.config/waybar/config`:

```
{
    "layer": "top",
    "modules-left": ["sway/workspaces", "sway/mode"],
    "modules-center": ["sway/window"],
    "modules-right": ["clock"],
    "sway/window": {
        "max-length": 50
    },
    "clock": {
        "format-alt": "{:%a, %d. %b  %H:%M}"
    }
}
```
