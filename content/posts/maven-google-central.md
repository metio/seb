---
title: Google Central
date: 2021-11-08
categories:
- snippet
tags:
- maven
- google
- repository
---

Some time ago, [Google](https://www.google.com/) started hosting a [copy](https://storage-download.googleapis.com/maven-central/index.html) of [Maven Central](https://search.maven.org/). Configure it in your `~/.m2/settings.xml` like this:

```xml
<settings>
  <mirrors>
    <mirror>
      <id>google-maven-central</id>
      <name>Google Maven Central (Asia)</name>
      <url>https://maven-central-asia.storage-download.googleapis.com/maven2/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
    <mirror>
      <id>google-maven-central</id>
      <name>Google Maven Central (EU)</name>
      <url>https://maven-central-eu.storage-download.googleapis.com/maven2/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
    <mirror>
      <id>google-maven-central</id>
      <name>Google Maven Central (US)</name>
      <url>https://maven-central.storage-download.googleapis.com/maven2/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
  </mirrors>
</settings>
```

Pick the mirror nearest to your location to get best speeds.
