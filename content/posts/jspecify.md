---
title: jspecify
date: 2023-01-09
categories:
- snippet
tags:
- java
- nullness
- jspecify
---

Every [Java](https://www.java.com/) developer has probably encountered a `NullPointerException` at least once in their life. The exception is thrown every time you try to dereference and use some object before initializing it. The following snippet shows a simple example:

```java
String someName;         // value is 'null'

someName.toUpperCase(); // throws NullPointerException
```

Modern [IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment) have some sort of detection for this kind of problem and warn developers while they are writing code like this. Those IDEs typically rely on static code analysis to determine if a value is `null` and therefore a potential for a `NullPointerException` is present in your code. To improve the result of such an analysis, annotations can be placed on your code which signal that a parameter can or can not be `null`. Multiple approaches have existed in the past to define a standard set of annotations for such a task, however none of them succeeded.

[jspecify](https://jspecify.dev/) is the latest approach that tries to establish a [standard](https://xkcd.com/927/). It has gained wide [community support](https://jspecify.dev/about) and recently celebrated their first public release (`0.3.0`).

The following snippet shows the dependency declaration for [Maven](https://maven.apache.org/) projects:

```xml
<dependencies>
    <dependency>
        <groupId>org.jspecify</groupId>
        <artifactId>jspecify</artifactId>
        <version>0.3.0</version>
    </dependency>
</dependencies>
```

In case you want to declare that nothing in your module can ever be `null`, place the `@NullMarked` on your `module-info.java` like this:

```java
@org.jspecify.annotations.NullMarked
module your.module.here {

    requires org.jspecify;

    // ...

}
```

The tooling support is not quiet clear yet, however if you are developing a library there is [no harm](https://github.com/jspecify/jspecify/wiki/adoption#should-my-library-adopt-these-annotations-now) in adding these annotations now and let your users enjoy their null-free life once tools have caught up.
