---
title: Short Git Clones
date: 2020-06-15
menu: topic
categories:
- snippet
tags:
- git
- clone
- ssh
---

In case you don't want to write `git clone git@github.com:orga/repo.git` all the time, consider using a custom SSH config like this:

```
Host github
    HostName github.com
    User git
    IdentityFile ~/.ssh/<KEY-FOR-GITHUB>

Host gitlab
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/<KEY-FOR-GITLAB>

Host bitbucket
    HostName bitbucket.org
    User git
    IdentityFile ~/.ssh/<KEY-FOR-BITBUCKET>

Host codeberg
    HostName codeberg.org
    User git
    IdentityFile ~/.ssh/<KEY-FOR-CODEBERG>
```

Once configured, you can now write:

```shell script
$ git clone github:orga/repo.git
$ git clone gitlab:orga/repo.git
$ git clone bitbucket:orga/repo.git
$ git clone codeberg:orga/repo.git
```
