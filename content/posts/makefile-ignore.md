---
title: Ignore Exit Codes
date: 2021-02-08
categories:
- snippet
tags:
- make
- Makefile
- exit code
---

In case you are using a `Makefile` to define a complex build step - e.g. start database, run tests, stop database - consider using the `-` qualifier in front of your actual build step like this:

```makefile
.PHONY: build
build:
	start-database
	-build-software
	stop-database
```

Thanks to `-`, the database will be stopped even if building your software fails, thus making sure to clean up after ourselves once the build finishes.

## Links

- https://www.gnu.org/software/make/manual/make.html#Recipe-Echoing
- https://www.gnu.org/software/make/manual/make.html#Errors-in-Recipes
- https://www.gnu.org/software/make/manual/make.html#How-the-MAKE-Variable-Works
