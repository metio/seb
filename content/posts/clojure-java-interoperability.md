---
title: Clojure Java Interoperability
date: 2022-01-29
categories:
- snippet
tags:
- clojure
- java
- interoperability
---

Clojure has several [forms and macros](http://clojure.org/java_interop) to call Java code. However, calling Clojure code from Java is not always so straightforward. The following post shows the different options currently available.

## Using `gen-class`

Clojure code can be [compiled](http://clojure.org/compilation) to standard JVM bytecode using [gen-class](http://clojure.github.io/clojure/clojure.core-api.html#clojure.core/gen-class).

### Adding static modifiers

Clojure imposes the notion of immutability. As such Clojure functions are/should be void of any state or side effects and only operate on the given input. Therefore, exporting Clojure functions as static Java methods makes sense. The following example defines a Clojure function, a corresponding Java-callable function and exports the Java function as a static method in the class `com.example.Computation`.

```clojure
(ns com.example.computation
  (:gen-class
    :name com.example.Computation
    :methods [#^{:static true} [incrementRange [int] java.util.List]]))

(defn increment-range
  "Creates a sequence of numbers up to max and then increments them."
  [max]
  (map inc (take max (range))))

(defn -incrementRange
  "A Java-callable wrapper around the 'increment-range' function."
  [max]
  (increment-range max))
```

The Java wrapper has to follow the [standard rules](https://docs.oracle.com/javase/specs/jls/se17/html/jls-3.html#jls-3.8) for method names. Thus `increment-range` has to be renamed to `incrementRange` (or some similar name without the "-" in it). The "-" prefix for the Java wrapper can be configured inside the `:gen-class` form and will be removed once `gen-class` runs. The usage from Java looks like this:

```java
package com.example

public class ClojureJavaInteropStatic {

    public static void main(String[] args) {
        List incrementedRange = Computation.incrementRange(10);
    }

}
```

### Adding generics

The returned list in the above code is raw because the method definition doesn't use generics. To solve this problem declare that the generated class `:implements` a certain interface that exposes the desired method definition(s). You won't be able to declare your methods as static anymore, but get a generified method for all your Java needs.

The Java interface:

```java
package com.example

public interface RangeIncrementer {
  List<Long> incrementRange(int max);
}
```

The changed Clojure namespace:

```clojure
(ns com.example.computation
  (:gen-class
    :name com.example.Computation
    :implements [com.example.RangeIncrementer]))

(defn increment-range
  "Creates a sequence of numbers up to max and then increments them."
  [max]
  (map inc (take max (range))))

(defn -incrementRange
  "A Java-callable wrapper around the 'increment-range' function."
  [this max]
  (increment-range max))
```

Finally, the generified usage from Java:

```java
package com.example

public class ClojureJavaInteropGenerics {

    public static void main(String[] args) {
        RangeIncrementer incrementer = new Computation();
        List<Long> incrementedRange = incrementer.incrementRange(10);
    }

}
```

Couple of notes for this as well: First the generated class still only returns the raw type (`List` instead of  `List<Integer>`). So instead of using the class, use the interface for the variable declaration (`RangeIncrementer incrementer = ..` instead of `Computation comp = ..`). The interface will return the non-raw `List`. Second the function definition for `-incrementRange` is now slightly different. It needs an additional parameter (`this`) which exposes the current instance to the generated class/method.

Returning an array of something is also possible with the following construct `"[Ljava.lang.Object;"`. Need a 2-dim array? Just use `"[[Ljava.lang.Object;"` (notice the extra `[`) and so on. However, be aware that the method return types have to match, e.g. you can't specify a return type of array if your Clojure function does not return an array. In the example above the call to `map` returns `LazySeq` which itself is a `java.util.List`. Therefore, the method declaration is valid, and you won't get any `ClassCastException` when calling `incrementRange` from Java.

### Make your life easier with macros

Instead of defining every Clojure function which should be exported twice (the real function + the Java wrapper), it is possible to use a macro to do that extra work automatically.

```clojure
(require '[clojure.string :as string)

(defn camel-case [input]
  (let [words (string/split input #"[\s_-]+")]
    (string/join (cons (string/lower-case (first words)) (map string/capitalize (rest words))))))

(defn java-name [clojure-name]
  (symbol (str "-" (camel-case (str clojure-name)))))

(defmacro defn* [name & declarations]
  (let [java-name (java-name name)]
    `(do (defn ~name ~declarations)
       (defn ~java-name ~declarations))))
```

The macro `defn*` replaces `defn` and automatically creates a second function with a valid camel-cased Java method name. The macro is available as a [small library](https://github.com/sebhoss/def-clj) at [Maven Central](https://search.maven.org/search?q=g:com.github.sebhoss%20a:def-clj). The macro won't add the extra parameter mentioned above to Java wrapper, so it is only useful for declaring static methods.

## Using the Clojure Runtime

Using `gen-class` imposes certain limitations on calling Clojure code from Java. One of those are functions which make use of Clojure [parameter destructuring](http://clojure.org/reference/special_forms#binding-forms). To invoke those functions you have to use the Clojure runtime.

```java
// The Clojure 'require' function from the 'clojure.core' namespace.
Var require = RT.var("clojure.core", "require");

// Your namespace
Symbol namespace = Symbol.intern("DESIRED.NAMESPACE.HERE");

// Your function
Var function = RT.var("DESIRED.NAMESPACE.HERE", "DESIRED-FUNCTION");

// The required keyword for the above function
Keyword keyword = Keyword.intern("REQUIRED-KEYWORD");

// Require/Import your namespace
require.invoke(namespace);

// Invoke your function with the given keyword and its value
Object result = function.invoke(keyword, VALUE);
```

The desired namespace has to be on the classpath for this to work. Alternatively it is possible to load an entire Clojure script, as shown in the following example:

```java
RT.loadResourceScript("DESIRED/NAMESPACE/HERE.clj");
RT.var("DESIRED.NAMESPACE.HERE", "DESIRED-FUNCTION").invoke(PARAMETER);
```

On a big project it is properly wise to move Java->Clojure interop code into helper classes/methods. Look [here](https://github.com/mikera/clojure-utils/blob/master/src/main/java/mikera/cljutils/Clojure.java) for an example.
