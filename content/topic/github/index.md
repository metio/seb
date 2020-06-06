---
title: GitHub
date: 2020-06-30
categories:
- snippets
tags:
- github
---

This page contains useful [GitHub](https://github.com/) snippets which are being used by the project at https://github.com/metio. 

## Manage default community health files

https://help.github.com/en/github/building-a-strong-community/creating-a-default-community-health-file

You can add default community health files to the root of a public repository called .github that is owned by an organization or user account.

GitHub will use and display default files for any public repository owned by the account that does not have its own file of that type in any of the following places:

- the root of the repository
- the .github folder
- the docs folder

For example, anyone who creates an issue or pull request in a public repository that does not have its own CONTRIBUTING file will see a link to the default CONTRIBUTING file. If a repository has any files in its own .github/ISSUE_TEMPLATE folder, including issue templates or a config.yml file, none of the contents of the default .github/ISSUE_TEMPLATE folder will be used.

Default files are not included in clones, packages, or downloads of individual repositories because they are stored only in the .github repository.
