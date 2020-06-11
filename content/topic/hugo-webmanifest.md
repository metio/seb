---
title: Web app manifests with Hugo
date: 2021-01-11
menu: topic
categories:
- snippet
- website
tags:
- hugo
- web app
- manifest
---

In order to publish a [web app manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest) document with your [Hugo](gohugo.io/) site, configure a [media type](https://en.wikipedia.org/wiki/Media_type) in your `config.toml`:

```toml
[mediaTypes."application/manifest+json"]
  suffixes = ["webmanifest"]
[outputFormats.Webmanifest]
  name = "Web App Manifest"
  mediaType = "application/manifest+json"
  baseName = "manifest"
  isPlainText = false
  rel = "alternate"
  isHTML = false
  noUgly = true
  permalinkable = false
```

Create a new layout, e.g. in `_default/home.manifest.json` with the following content:

```gotemplate
{
  "name": "{{ .Site.Title }}",
  "short_name": "{{ .Site.Title }}",
  "start_url": ".",
  "display": "minimal-ui",
  "background_color": "#fff",
  "description": "{{ .Site.Params.description }}"
}
```

## Links

- https://web.dev/add-manifest/
