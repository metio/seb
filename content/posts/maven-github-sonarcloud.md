---
title: Analyse Maven projects with SonarCloud using GitHub Actions
date: 2021-05-03
categories:
- devops
- snippet
tags:
- maven
- github
- sonarqube
- github actions
---

In order to analyse [Maven](https://maven.apache.org/) projects with [SonarCloud](https://sonarcloud.io) using [GitHub Actions](https://github.com/features/actions), first create the following `settings.xml` file:

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">

    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>

    <activeProfiles>
        <activeProfile>sonar</activeProfile>
    </activeProfiles>

    <profiles>
        <profile>
            <id>sonar</id>
            <properties>
                <sonar.host.url>https://sonarcloud.io</sonar.host.url>
                <sonar.organization>YOUR_ORG</sonar.organization>
                <sonar.projectKey>YOUR_PROJECT</sonar.projectKey>
                <sonar.login>${env.SONAR_TOKEN}</sonar.login>
            </properties>
        </profile>
    </profiles>
</settings>
```

Finally, add a step to your workflow:

```yaml
- name: Verify Project
  run: mvn --settings $GITHUB_WORKSPACE/settings.xml verify sonar:sonar
  env:
    SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

## Links

- https://maven.apache.org/settings.html
- https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-maven/
