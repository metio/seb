---
title: Lock your screen on SwayWM
date: 2021-04-05
menu: topic
categories:
- snippet
tags:
- swaywm
- screenlock
---

[SwayWM](https://swaywm.org/) uses can use a mixture of [swaylock](https://github.com/swaywm/swaylock) to lock their screen. Place the following key binding in your Sway config:

```shell script
#
# Screen lock
#
bindsym $mod+Ctrl+l exec swaylock -c 000000
```

`$mod+Ctrl+l` will lock your screen and turn it to black.
