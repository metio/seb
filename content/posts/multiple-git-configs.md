---
title: Multiple Git Configurations
date: 2023-01-05
categories:
- snippet
tags:
- git
- config
---

To split yet re-use as much configuration for Git as possible, you can create one root configuration that looks similar to this:

```ini
[user]
  name = Your Name Here

[includeIf "gitdir:~/git/personal/"]
  path = ~/.config/git/personal
[includeIf "gitdir:~/git/work/"]
  path = ~/.config/git/work
```

The [includeIf](https://git-scm.com/docs/git-config#_includes) directive supports multiple keywords. In my case, work and personal projects have a different root directory, therefore I can filter based on the location using `gitdir`. The personal Git configuration simply looks like this:

```ini
[user]
  email = personal.email@example.com
```

and the work related configuration like this using a different email address:

```ini
[user]
  email = first.last@work.example
```

Additional settings that are different for personal/work accounts can be split the same way, for example to use a different signing key for work.
