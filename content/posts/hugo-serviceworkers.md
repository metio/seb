---
title: Service workers with Hugo
date: 2021-01-25
categories:
- snippet
- website
tags:
- hugo
- service worker
---

To use a [serviceworker](https://serviceworke.rs/) to cache a [Hugo](https://gohugo.io/) site, configure a [media type](https://en.wikipedia.org/wiki/Media_type) in your `config.toml`:

```toml
[mediaTypes."application/javascript"]
  suffixes = ["js"]
[outputFormats.ServiceWorker]
  name = "ServiceWorker"
  mediaType = "application/javascript"
  baseName = "serviceworker"
  isPlainText = false
  rel = "alternate"
  isHTML = false
  noUgly = true
  permalinkable = false
```

Create a new layout in `_default/home.serviceworker.js` with the following content:

```gotemplate
const CACHE = 'cache-and-update';

self.addEventListener('install', (event) => {
  event.waitUntil(precache());
});

self.addEventListener('fetch', (event) => {
  event.respondWith(fromCache(event.request));
  event.waitUntil(update(event.request));
});

const precache = async () => {
    const cache = await caches.open(CACHE);
    return await cache.addAll([
        {{ range $i, $e := .Site.RegularPages }}
        '{{ $.RelPermalink }}'{{ if $i }}, {{ end }}
        {{ end }}
    ]);
}

const fromCache = async (request) => {
    const cache = await caches.open(CACHE);
    const match = await cache.match(request);
    return match || Promise.reject('no-match');
}

const update = async (request) => {
    const cache = await caches.open(CACHE);
    const response = await fetch(request);
    return await cache.put(request, response);
}
```

## Links

- https://github.com/gohugoio/hugo/issues/5495
- https://github.com/wildhaber/offline-first-sw
- https://gohugohq.com/howto/go-offline-with-service-worker/
