---
title: Makefile Help
date: 2020-06-29
categories:
- snippet
tags:
- make
- Makefile
- help
- perl
---

Use the following [Perl](https://www.perl.org/) snippet to automatically generate help output for your `Makefile`:

```makefile
GREEN  := $(shell tput -Txterm setaf 2)
WHITE  := $(shell tput -Txterm setaf 7)
YELLOW := $(shell tput -Txterm setaf 3)
RESET  := $(shell tput -Txterm sgr0)

HELP_FUN = \
    %help; \
    while(<>) { push @{$$help{$$2 // 'targets'}}, [$$1, $$3] if /^([a-zA-Z\-]+)\s*:.*\#\#(?:@([a-zA-Z\-]+))?\s(.*)$$/ }; \
    print "usage: make [target]\n\n"; \
    for (sort keys %help) { \
    print "${WHITE}$$_:${RESET}\n"; \
    for (@{$$help{$$_}}) { \
    $$sep = " " x (32 - length $$_->[0]); \
    print "  ${YELLOW}$$_->[0]${RESET}$$sep${GREEN}$$_->[1]${RESET}\n"; \
    }; \
    print "\n"; }
```

In order to use `HELP_FUN`, add the following `help` target to the same `Makefile`:

```makefile
.DEFAULT_GOAL := help

.PHONY: help
help: ##@other Show this help
	@perl -e '$(HELP_FUN)' $(MAKEFILE_LIST)
```

Each target in the `Makefile` is marked as [phony](https://www.gnu.org/software/make/manual/html_node/Phony-Targets.html) to signal that those targets are not actually files that are generated as part of your build process. The optional description of a target can be placed after the `##@` prefix. The first word represents the group of a target and everything that follows is the description of a target. All targets should be formatted just like the `help` target:

```makefile
.PHONY: compile
compile: ##@hacking Compile your code
	<compile some code>

.PHONY: test
test: ##@hacking Test your code
	<test some code>

.PHONY: sign-cla
sign-cla: ##@contrib Sign the contributor license agreement
	<sign some file>
```

Once in place, you can either use `make` without any argument to call the `help` target or use `make help` to see the generated output:

```shell script
$ make
usage: make [target]

contrib:
  sign-cla            Sign the contributor license agreement

hacking:
  compile             Compile your code
  test                Test your code

other:
  help                Show this help
```
