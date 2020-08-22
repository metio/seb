---
title: Firefox with dark mode
draft: true
menu: topic
categories:
- snippet
tags:
- firefox
- css
- dark mode
---

Nowadays websites can detect the preferred color scheme of an user with something like:

```css
@media (prefers-color-scheme: dark) {
    /* rules for dark mode */
}
```

In order to tell Firefox to usw dark mode for all webpages on linux, go to `about:config` and add/change ` ui.systemUsesDarkTheme` to `1` and `browser.in-content.dark-mode` to `true`.
