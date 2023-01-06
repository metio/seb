---
title: Specify encoding for Maven projects
date: 2021-05-31
categories:
- devops
- snippet
tags:
- maven
- encoding
- windows
- linux
- mac
---

In order to create [Maven](https://maven.apache.org/) projects which can be build on platforms like Linux/Mac/Windows, make sure to specify the encoding of your source code and resource files like this:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
</properties>
```

## Links

- https://maven.apache.org/pom.html#Properties
