---
title: Create GitHub releases with GitHub Actions
date: 2020-10-05
categories:
- devops
- snippet
tags:
- github
- github actions
- release
---

The [actions/create-release](https://github.com/actions/create-release) action allows to create a new [GitHub releases](https://help.github.com/en/github/administering-a-repository/about-releases) in your [GitHub Action](https://github.com/features/actions).

```yaml
name: <PIPELINE>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
     - name: Create Release
       uses: actions/create-release@v1
       env:
         GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
       with:
         tag_name: <TAG>
         release_name: <RELEASE>
         draft: false
         prerelease: false
         body: |
           Your release text here

           Some code block:
           ```yaml
           yaml:
             inside:
               of:
                 another: yaml
           ```
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<TAG>`: The Git tag to create.
- `<RELEASE>`: The release name to use.
