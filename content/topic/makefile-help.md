---
title: Makefile Help Target
date: 2020-06-29
menu: topic
categories:
- snippets
tags:
- make
- Makefile
- help
---

Use the following snippet to generate help output for your `Makefile`:

```makefile
.DEFAULT_GOAL := help

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

.PHONY: help
help: ##@other Show this help
	@perl -e '$(HELP_FUN)' $(MAKEFILE_LIST)
```

Each target in the `Makefile` is marked as [phony](https://www.gnu.org/software/make/manual/html_node/Phony-Targets.html) to signal that those targets are not actually files that are generated as part of your build process. The optional description of a target can be placed after the `##@` prefix. All other targets should be formatted just like the `help` target:

```makefile
.PHONY: compile
compile: ##@hacking Compile your code
	<compile some code>
```

Once in place, you can either use `make` without any argument to call the `help` target or use `make help` to see the following output:

```shell script
$ make
usage: make [target]

hacking:
  compile             Compile your code

other:
  help                Show this help
```
