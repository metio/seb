---
title: Bundling with Hugo
date: 2020-12-28
menu: topic
categories:
- snippets
- website
tags:
- hugo
- bundle
- assets
---

[Hugo](gohugo.io/) allows [bundling](https://gohugo.io/hugo-pipes/bundling/) of assets with several built-in functions:

```gotemplate
{{ $normalize := resources.Get "/css/normalize.css" }}
{{ $font := resources.Get "/css/font.css" }}
{{ $header := resources.Get "/css/header.css" }}
{{ $footer := resources.Get "/css/footer.css" }}
{{ $navigation := resources.Get "/css/navigation.css" }}
{{ $navigation_mobile := resources.Get "/css/navigation-mobile.css" }}
{{ $layout := resources.Get "/css/layout.css" }}
{{ $layout_mobile := resources.Get "/css/layout-mobile.css" }}
{{ $syntax := resources.Get "/css/syntax.css" }}
{{ $darkmode := resources.Get "/css/darkmode.css" | resources.Minify | resources.Fingerprint "sha512" }}

{{ $base := slice $normalize $font $header $footer $navigation $layout $syntax | resources.Concat "css/base.css" | resources.Minify | resources.Fingerprint "sha512" }}
{{ $mobile := slice $navigation_mobile $layout_mobile | resources.Concat "css/mobile.css" | resources.Minify | resources.Fingerprint "sha512" }}

<link href="{{ $base.Permalink }}" integrity="{{ $base.Data.Integrity }}" media="screen" rel="stylesheet">
<link href="{{ $mobile.Permalink }}" integrity="{{ $mobile.Data.Integrity }}" media="screen and (max-width: 800px)" rel="stylesheet">

<link href="{{ $darkmode.Permalink }}" integrity="{{ $darkmode.Data.Integrity }}" media="screen and (prefers-color-scheme: dark)" rel="stylesheet">
```
