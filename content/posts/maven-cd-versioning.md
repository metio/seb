---
title: Continuous Versioning with Maven
date: 2021-12-06
categories:
- snippet
- cd
tags:
- maven
- versioning
---

To automatically version [Maven](https://maven.apache.org/) projects, I like to use the [m-versions-p](https://www.mojohaus.org/versions-maven-plugin/) like this:

```shell script
$ mvn versions:set -DnewVersion=my.new.version -DgenerateBackupPoms=false
```

This will update the `version` property of every module in the reactor, e.g. to prepare them for the next release. In case you are using [GitHub Actions](https://github.com/features/actions), consider using a [timestamp](../create-timestamp-with-github-actions).
