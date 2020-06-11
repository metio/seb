---
title: Delay GitHub Actions builds
date: 2021-02-22
menu: topic
categories:
- devops
- snippet
tags:
- github
- github actions
- schedule
---

In order to delay the execution of an [GitHub Action](https://github.com/features/actions), use a mixture of the `on: schedule: ...` config, and a conditional build step.

```yaml
name: <PIPELINE>
on:
  schedule:
    - cron: '<CRON>'
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Count commits in last week
        id: commits
        run: echo "::set-output name=count::$(git rev-list --count HEAD --since='<DATE>')"
      - name: Build project
        if: steps.commits.outputs.count > 0
        run: build-project
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<CRON>`: Cron expression - use https://crontab.guru/.
- `<DATE>`: Git date expression that matches `<CRON>`.
