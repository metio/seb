---
title: Use a specific Hugo version with GitHub Actions
date: 2020-08-24
menu: topic
categories:
- devops
- snippet
tags:
- github
- github actions
- hugo
---

The [actions-hugo](https://github.com/peaceiris/actions-hugo) action allows to use a specific [Hugo](https://gohugo.io/) version in your [GitHub Action](https://github.com/features/actions).

```yaml
name: <PIPELINE>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Setup hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: <HUGO_VERSION>
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<HUGO_VERSION>`: The [released versions](https://github.com/gohugoio/hugo/releases) or use `latest` to always use the latest version of Hugo.
