---
title: multiple git configurations
date: 2023-01-05
menu: topic
categories:
- snippet
tags:
- git
- config
---

In order to split yet re-use as much configuration for Git as possible, you can create one root configuration that looks similar to this:

```ini
[user]
  name = user_name

[includeIf "gitdir:~/git/personal/"]
  path = ~/.config/git/personal
[includeIf "gitdir:~/git/work/"]
  path = ~/.config/git/work
```

The [includeIf](https://git-scm.com/docs/git-config#_includes) directive supports multiple matchers. In my case, work and personal projects have a different root directory, thus I can filter based on that alone. The personal Git config simply looks like this:

```ini
[user]
  email = personal.email@example.com
```

and the work related config like this using a different email address:

```ini
[user]
  email = first.last@work.example
```
