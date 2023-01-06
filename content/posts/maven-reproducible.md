---
title: Creating reproducible artifacts with Maven
date: 2021-05-17
menu: topic
categories:
- devops
- snippet
tags:
- maven
- reproducible
---

In order to create [reproducible builds](https://reproducible-builds.org/) with [Maven](https://maven.apache.org/) projects, it's enough to specify the `project.build.outputTimestamp` property like this:

```xml
<properties>
    <project.build.outputTimestamp>2020</project.build.outputTimestamp>
</properties>
```

## Links

- https://maven.apache.org/pom.html#Properties
- https://maven.apache.org/guides/mini/guide-reproducible-builds.html
- https://github.com/rodiontsev/maven-build-info-plugin
- https://github.com/phax/ph-buildinfo-maven-plugin
- https://github.com/Zlika/reproducible-build-maven-plugin
