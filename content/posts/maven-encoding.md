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

[Maven](https://maven.apache.org/) projects by default use the file encoding of the operating system. This can be problematic in case different operating systems with different encoding settings are used to build the project. Specify the encoding of your source code and resource files as in the following snippets to fix that problem.

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
</properties>
```

## Links

- https://maven.apache.org/pom.html#Properties
