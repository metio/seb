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

In case you don't want to write `git clone git@github.com:orga/repo.git` all the time, consider using a custom SSH config (`~/.ssh/config`) like this:

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
$ git clone github:orga/repo
$ git clone gitlab:orga/repo
$ git clone bitbucket:orga/repo
$ git clone codeberg:orga/repo
```

In case you are working with lots of repositories inside a single organization, consider adding the following git configuration (`~/.gitconfig`):

```
[url "github:orga/"]
  insteadOf = orga:
``` 

Which allows you to just write `git clone orga:repo`. Git will substitute `orga:` with `github:orga/`, therefore execute `git clone github:orga/repo`. Since we are using SSH to perform the clone, SSH will use its own config to figure out that `github` is an alias for `github.com`.
