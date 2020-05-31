---
title: Gitlab the Git Distributor
date: 2020-09-30
categories:
- configuration
tags:
- git
- gitlab
- github
- codeberg
- bitbucket
- repo.or.cz
---

[Git](https://git-scm.com/) at its core is a decentralized version control system. Yet a lot of people are relying on a single central server (github.com at the time of this writing) to share their work with others. [Pro Git](https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows) rightfully mentions it at first position in its chapter about distributed workflows because using a central server is usually the simplest approach to sharing code.

While the central server approach is easy to use, it might not work in all scenarios:

1. An enterprise wants to share internal code as part of an open source project. All development is happening internally, and the public mirror gets an occasional update once in a while.
2. In order to protect against outages of the central server, mirrors should be created and be kept up-to-date.

In case of the first scenario, tools like [copybara](https://github.com/google/copybara), [repoSpanner](https://github.com/repoSpanner/repoSpanner), or [distributed-git-forks](https://github.com/google/distributed-git-forks) offer a wide range of features to cover most details.

The second scenario can be solved with [Gitlab](https://gitlab.com)'s [mirror feature](https://docs.gitlab.com/ee/user/project/repository/repository_mirroring.html) quite easy. It allows to create a single pull-mirror and multiple push-mirrors. Thus, it can be used to pull from your central server and push into all mirrors.

![Code Flow](/image/git-distributor.svg)

```
               +----------------+               
               |     GitHub     |               
               +----------------+               
                        ^                       
                        |                       
                        |                       
               +----------------+               
         +-----|     Gitlab     |------+        
         |     +----------------+      |        
         |                             |        
         |                             |        
         v                             v        
+----------------+            +----------------+
|    Codeberg    |            |    BitBucket   |
+----------------+            +----------------+
```

1. Go to `Settings > Repository` and expand `Mirroring repositories`
2. Enter the URL of your central Git repository as the pull source, e.g. `https://github.com/metio/krei.git`
3. Enter one push target for each mirror. Since pushing usually requires authentication, make sure the URL of the mirror contains a username, e.g. `https://YOUR_USER@codeberg.org/metio.wtf/krei.git`. Add an access token for each mirror and select `password` as authentication method.

In case you prefer SSH keys over HTTP access tokens, just select `SSH public key` as authentication method and make sure your key is both saved in Gitlab and all mirrors.
