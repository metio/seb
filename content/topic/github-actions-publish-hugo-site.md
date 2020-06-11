---
title: Publish Hugo site with GitHub Actions
date: 2020-09-21
menu: topic
categories:
- devops
- snippet
tags:
- github
- github actions
- publish
- hugo
---

The [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages) action allows to publish a [Hugo](https://gohugo.io/) site in your [GitHub Action](https://github.com/features/actions).

```yaml
name: <PIPELINE>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Deploy Website
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: <PUBLISH_DIR>
          force_orphan: true
          cname: <CNAME>
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<PUBLISH_DIR>`: The file system location of the built site.
- `<CNAME>`: The `CNAME` of your custom domain.
