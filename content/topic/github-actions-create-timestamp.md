---
title: Create Timestamp with GitHub Actions
date: 2020-06-30
menu: topic
categories:
- devops
- cd
- snippets
tags:
- github
- github actions
- timestamp
---

In case you are into [calver](https://calver.org/) or have another reason to create a timestamp with GitHub Actions, do the following:

```yaml
name: <PIPELINE>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Create release version
        id: <ID>
        run: echo "::set-output name=<NAME>::$(date +'%Y.%m.%d-%H%M%S')"
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<ID>`: The unique ID of the timestamp step.
- `<NAME>`: The name of the created timestamp.

The special syntax `::set-output name=<NAME>::` declares that the output of the command (`echo`) should be saves to a variable called `<NAME>`. Together with the `id` of the pipeline step, this value can be referenced like this `${{ steps.<ID>.outputs.<NAME> }}` in the next steps of your pipeline.