---
title: Mirror Git Repositories
date: 2021-06-28
categories:
- snippet
- decentralized
tags:
- git
- push
- mirror
---

In case you want to make use of the decentralized nature of [Git](https://git-scm.com/), consider using multiple push targets like this:

```shell script
$ git remote set-url origin --push --add git@example.com/project.git
$ git remote set-url origin --push --add git@another.com/project.git
```

Note that the first call to `set-url` will overwrite an existing remote creating with `git clone`. Any additional call will actually recognize the `--add` option and add the new target to an existing remote. 

## Links

- [push only mirrors](../push-only-mirrors-for-git-repositories)
- [gitlab-distributor](../gitlab-the-git-distributor)
