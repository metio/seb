---
title: GitHub Actions
date: 2020-06-30
categories:
- snippets
tags:
- github
- github actions
---

This page contains useful [GitHub Actions](https://github.com/features/actions) snippets and workflows that are being used by the project at https://github.com/metio. All snippets share a few variables:

- `<NAME>`: The name of your pipeline.
- `<RUN_ON>`: The runner to use, see GitHub's own [documentation](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) for possible values.
- `<PROJECT>`: The name of your project.

## Create a timestamped release version

```yaml
name: <NAME>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Create release version
        id: release
        run: echo "::set-output name=version::$(date +'%Y.%m.%d-%H%M%S')"
```

The special syntax `::set-output name=version::` declares that the output of the command (`echo`) should be saves as version. Together with the `id` of the pipeline step, this value can be referenced like this `${{ steps.release.outputs.version }}` in the next steps of your pipeline.

See https://calver.org/ for timestamp/calendar versioning.

## Setup a specific Java version

Uses https://github.com/actions/setup-java.

```yaml
name: <NAME>
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Set up JDK <JDK_VERSION>
        uses: actions/setup-java@v1
        with:
          java-version: <JDK_VERSION>
```

Replace `<JDK_VERSION>` with the required Java version for your project.

## Setup a specific Hugo version

```yaml
name: <NAME>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Setup hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: <HUGO_VERSION>
```

Replace `<HUGO_VERSION>` with one of the [released versions](https://github.com/gohugoio/hugo/releases) or use `latest` to always use the latest version of Hugo.

## Cache Maven downloads

Uses https://github.com/actions/cache.

```yaml
name: <NAME>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Cache all Maven artifacts
        uses: actions/cache@v1
        with:
          path: ~/.m2/repository
          key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
          restore-keys: |
            ${{ runner.os }}-maven-
```

## Deploy to GitHub Pages

Uses https://github.com/peaceiris/actions-gh-pages.

```yaml
name: <NAME>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Deploy Website
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./krei-docs/public
          force_orphan: true
          cname: krei.projects.metio.wtf
```

## Create a GitHub release

Uses https://github.com/actions/create-release.

```yaml
name: <NAME>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
     - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{steps.release.outputs.version}}
          release_name: Release ${{steps.release.outputs.version}}
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

## Upload an asset to the latest GitHub release

Uses https://github.com/actions/upload-release-asset.

```yaml
name: <NAME>
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

## Send an email

Uses https://github.com/dawidd6/action-send-mail.

```yaml
name: <NAME>
jobs:
  build:
    runs-on: <RUN_ON>
    steps:
      - name: Send mail
        uses: dawidd6/action-send-mail@v2
        with:
          server_address: ${{ secrets.MAIL_SERVER }}
          server_port: ${{ secrets.MAIL_PORT }}
          username: ${{ secrets.MAIL_USERNAME }}
          password: ${{ secrets.MAIL_PASSWORD }}
          subject: <PROJECT> release ${{ steps.release.outputs.version }}
          body: See https://github.com/metio/<PROJECT>/releases/tag/${{ steps.release.outputs.version }} for details.
          to: ${{ secrets.MAIL_RECIPIENT }}
          from: ${{ secrets.MAIL_SENDER }}
```

Create appropriate secret in your organization or project. In case you are using an organization, but different mailing lists, define `MAIL_RECIPIENT` for each project.

## Send Mastodon toot

Uses https://github.com/rzr/fediverse-action.

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
          message: "#<PROJECT> version ${{ steps.release.outputs.version }} published! https://github.com/metio/<PROJECT>/releases/tag/${{ steps.release.outputs.version }} #metio"
          host: ${{ secrets.MASTODON_SERVER }}
```
