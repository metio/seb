---
title: Firefox with dark mode
draft: true
categories:
- snippet
tags:
- firefox
- css
- dark mode
---

Nowadays websites can detect the preferred color scheme of a user with something like:

```css
@media (prefers-color-scheme: dark) {
    /* rules for dark mode */
}
```

To tell Mozilla Firefox to use dark mode for all web pages on Linux, go to `about:config` and add/change ` ui.systemUsesDarkTheme` to `1` and `browser.in-content.dark-mode` to `true`.
