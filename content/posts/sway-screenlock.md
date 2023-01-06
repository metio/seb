---
title: Lock your screen on SwayWM
date: 2021-04-05
categories:
- snippet
tags:
- swaywm
- screenlock
---

[SwayWM](https://swaywm.org/) users can use [swaylock](https://github.com/swaywm/swaylock) to lock their screen. Place the following key binding in your Sway configuration:

```shell
# lock your screen
bindsym $mod+Ctrl+l exec swaylock --color 000000
```

`$mod+Ctrl+l` will lock your screen and turn it to black. The `--color` flag allows any color in the form of `rrggbb[aa]`.
