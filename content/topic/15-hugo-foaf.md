---
title: FOAF with Hugo
date: 2020-11-30
menu: topic
categories:
- snippet
- website
tags:
- hugo
- foaf
---

In order to publish a [FOAF](http://www.foaf-project.org/) document with your [Hugo](https://gohugo.io/) site, configure a [media type](https://en.wikipedia.org/wiki/Media_type) in your `config.toml`:

```toml
[mediaTypes."application/rdf+xml"]
  suffixes = ["rdf"]
[outputFormats.Foaf]
  name = "FOAF"
  mediaType = "application/rdf+xml"
  baseName = "foaf"
  isPlainText = false
  rel = "alternate"
  isHTML = false
  noUgly = true
  permalinkable = false
```

Create a new layout, e.g. in `_default/home.foaf.rdf` with the following content:

```gotemplate
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:rdfs="http://www.w3.org/2000/01/rdf-schema#" xmlns:foaf="http://xmlns.com/foaf/0.1/">
  <foaf:PersonalProfileDocument rdf:about="">
    <foaf:maker rdf:resource="#me" />
    <foaf:primaryTopic rdf:resource="{{ .Site.Title }}" />
  </foaf:PersonalProfileDocument>

  <foaf:Project rdf:ID="{{ .Site.Title }}">
    <foaf:name>{{ .Site.Title }}</foaf:name>
    <foaf:homepage rdf:resource="{{ .Site.BaseURL }}" />
  </foaf:Project>

  {{ range $.Site.Data.contributors }}
  <foaf:Person rdf:ID="{{ .id }}">
    <foaf:name>{{ .first_name }} {{ .last_name }}</foaf:name>
    <foaf:title>{{ .title }}</foaf:title>
    <foaf:givenname>{{ .first_name }}</foaf:givenname>
    <foaf:family_name>{{ .last_name }}</foaf:family_name>
    <foaf:mbox rdf:resource="mailto:{{ .email }}" />
    <foaf:homepage rdf:resource="{{ .website }}" />
  </foaf:Person>
  {{ end }}
</rdf:RDF>
```
