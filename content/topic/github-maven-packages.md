---
title: GitHub Packages with Maven
date: 2021-11-22
menu: topic
categories:
- snippet
tags:
- maven
- github
- github packages
---

[GitHub Packages](https://github.com/features/packages) can be used to host [Maven](https://maven.apache.org/) packages with the following configuration in your `~/.m2/settings.xml`:

```xml
<settings>
  <profiles>
    <profile>
      <id>github</id>
      <repositories>
        <repository>
          <id>maven-build-process</id>
          <name>GitHub maven-build-process Apache Maven Packages</name>
          <url>https://maven.pkg.github.com/metio/maven-build-process</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </repository>
        <repository>
          <id>hcf4j</id>
          <name>GitHub hcf4j Apache Maven Packages</name>
          <url>https://maven.pkg.github.com/metio/hcf4j</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </repository>
      </repositories>
    </profile>
  </profiles>
  <servers>
    <server>
      <id>maven-build-process</id>
      <username>USERNAME</username>
      <password>GITHUB_TOKEN</password>
    </server>
    <server>
      <id>hcf4j</id>
      <username>USERNAME</username>
      <password>GITHUB_TOKEN</password>
    </server>
  </servers>
</settings>
```

You will have to add another repository/server for each project you are fetching from GitHub.
