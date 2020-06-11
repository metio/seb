---
title: Executable READMEs
date: 2021-03-08
menu: topic
categories:
- snippet
tags:
- make
- Makefile
- README
- teamwork
---

`README` file typically contain information about the project itself, e.g. how it can be installed/used/build. Most of the time these files contains command line instructions that users/contributors copy and paste into their terminal. Instead of doing that, consider placing a `Makefile` in the root of your project which contains the exact same instructions. Thanks to `make`, all your contributors can now use TAB-completion to run any of the pre-defined `make` targets.

The following example is part of one of my projects, and I certainly don't want to type (or even copy) that all the time:

```makefile
.PHONY: release-into-local-nexus
release-into-local-nexus:
	mvn versions:set \
	   -DnewVersion=$(TIMESTAMPED_VERSION) \
	   -DgenerateBackupPoms=false
	-mvn clean deploy scm:tag \
	   -DpushChanges=false \
	   -DskipLocalStaging=true \
	   -Drelease=local
	mvn versions:set \
	   -DnewVersion=9999.99.99-SNAPSHOT \
	   -DgenerateBackupPoms=false
```

With the above target in place, everyone can now do `make release-into-local-nexus` instead of typing/copying the commands themselves. Thanks to TAB-completion you just have to do `make r<TAB>` and confirm with `>ENTER>` to perform a release.

## Links

- https://www.makeareadme.com/
- https://opensource.guide/starting-a-project/#launching-your-own-open-source-project
