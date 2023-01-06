---
title: Waybar on SwayWM
date: 2021-08-23
categories:
- snippet
tags:
- swaywm
- waybar
---

[Waybar](https://github.com/Alexays/Waybar/) can be used as a status bar for [SwayWM](https://swaywm.org/). You tell Sway to use it with the following snippet in your Sway configuration:

```
bar {
    swaybar_command waybar
}
```

Configure Waybar itself in `~/.config/waybar/config`:

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
