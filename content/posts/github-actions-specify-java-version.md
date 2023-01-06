---
title: Use a specific Java version with GitHub Actions
date: 2020-08-10
menu: topic
categories:
- devops
- snippet
tags:
- github
- github actions
- java
---

The [setup-java](https://github.com/actions/setup-java) action allows to use a specific Java version in your [GitHub Action](https://github.com/features/actions).

```yaml
name: <PIPELINE>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Set up JDK <JDK_VERSION>
        uses: actions/setup-java@v1
        with:
          java-version: <JDK_VERSION>
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<JDK_VERSION>`: The required Java version for your project.
