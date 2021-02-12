---
title: Send toots with GitHub Actions
date: 2020-11-16
menu: topic
categories:
- devops
- snippet
tags:
- github
- github actions
- toot
- mastodon
---

The [rzr/fediverse-action](https://github.com/rzr/fediverse-action) action allows to send a [toot](https://joinmastodon.org/) in your [GitHub Action](https://github.com/features/actions).

```yaml
name: <NAME>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Publish Toot
        uses: rzr/fediverse-action@master
        with:
          access-token: ${{ secrets.MASTODON_TOKEN }}
          message: <MESSAGE>
          host: ${{ secrets.MASTODON_SERVER }}
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<MESSAGE>`: Message for the toot.
