---
title: Short Git Clones
date: 2020-06-15
categories:
- snippet
tags:
- git
- clone
- ssh
---

In case you don't want to write `git clone git@github.com:orga/repo.git` all the time, consider using a custom SSH configuration (`~/.ssh/config`) like this:

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

In case you are working with many repositories inside a single organization, consider adding the following Git configuration (`$XDG_CONFIG_HOME/git/config` or `~/.gitconfig`):

```
[url "github:orga/"]
  insteadOf = orga:
[url "gitlab:orga/"]
  insteadOf = orgl:
[url "bitbucket:orga/"]
  insteadOf = orgb:
[url "codeberg:orga/"]
  insteadOf = orgc:
``` 

Which allows you to just write:

```shell script
$ git clone orga:repo
$ git clone orgl:repo
$ git clone orgb:repo
$ git clone orgc:repo
``` 
 
Git will substitute the `insteadOf` values like `orga:` with the configured `url` (for example `github:orga/`). The actual clone URL is `github:orga/repo` at this point, which can be used by Git together with the SSH configuration mentioned above to clone repositories.
