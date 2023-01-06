---
title: Upload release assets with GitHub Actions
date: 2020-10-19
categories:
- devops
- snippet
tags:
- github
- github actions
- release
- asset
- upload
---

The [actions/upload-release-asset](https://github.com/actions/upload-release-asset) action allows to upload a [release artifact](https://developer.github.com/v3/repos/releases/#upload-a-release-asset) in your [GitHub Action](https://github.com/features/actions).

```yaml
name: <PIPELINE>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Upload Release Asset
        id: upload_release_asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./some/path/to/file.zip
          asset_name: public-name-for-file.zip
          asset_content_type: application/zip
```

- `<PIPELINE>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
