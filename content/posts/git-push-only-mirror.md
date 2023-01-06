---
title: Push-only mirrors for Git Repositories
date: 2021-07-12
categories:
- snippet
- decentralized
tags:
- git
- push
- mirror
---

In case you want to have push-only mirrors for your [Git](https://git-scm.com/) repository, consider adding a special mirror remote like this:

```shell script
$ git remote add mirrors DISABLED
$ git remote set-url --add --push mirrors git@codeberg.org:org/repo.git
$ git remote set-url --add --push mirrors git@gitlab.com:org/repo.git
$ git remote set-url --add --push mirrors git@bitbucket.org:org/repo.git
```

The above will create a new remote called `mirrors` which has no `fetch` URL and therefore can only be pushed:

```shell script
$ git remote -v
mirrors DISABLED (fetch)
mirrors git@codeberg.org:org/repo.git (push)
mirrors git@gitlab.com:org/repo.git (push)
mirrors git@bitbucket.org:org/repo.git (push)
```

Calling `git push mirrors main:main` will push the local `main` branch into all defined mirrors.

## Links

- [git mirror](../mirror-git-repositories)
- [gitlab-distributor](../gitlab-the-git-distributor)
