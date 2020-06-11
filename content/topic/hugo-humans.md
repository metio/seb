---
title: humans.txt with Hugo
date: 2020-12-14
menu: topic
categories:
- snippets
- website
tags:
- hugo
- 'humans.txt'
---

In order to publish a [humans.txt](http://humanstxt.org/) document with your [Hugo](gohugo.io/) site, configure a [media type](https://en.wikipedia.org/wiki/Media_type) in your `config.toml`:

```toml
[mediaTypes."text/plain"]
  suffixes = ["txt"]
[outputFormats.Humans]
  name = "Humans"
  mediaType = "text/plain"
  baseName = "humans"
  isPlainText = true
  rel = "alternate"
  isHTML = false
  noUgly = true
  permalinkable = false
```

Create a new layout, e.g. in `_default/home.humans.txt` with the following content:

```gotemplate
/* TEAM */
{{ range $.Site.Data.contributors }}
{{ .title }}: {{ .first_name }} {{ .last_name }}
Site: {{ .website }}
{{ end }}
```
