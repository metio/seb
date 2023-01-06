---
title: Cache Maven artifacts with GitHub Actions
date: 2020-09-07
categories:
- devops
- snippet
tags:
- github
- github actions
- cache
- maven
---

The [actions/cache](https://github.com/peaceiris/actions-hugo) action allows to cache artifacts in your [GitHub Action](https://github.com/features/actions).

```yaml
name: <PIPELINE>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Cache Maven artifacts
        uses: actions/cache@v1
        with:
          path: ~/.m2/repository
          key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
          restore-keys: |
            ${{ runner.os }}-maven-
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
