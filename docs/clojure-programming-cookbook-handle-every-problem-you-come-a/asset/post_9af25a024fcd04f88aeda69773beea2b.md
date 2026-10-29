
Using the doto macro makes the code simpler:

```
(let [hash (java.util.HashMap.)]
    (.put hash "key1" "value1")
    (.put hash "key2" "value2")
    (.put hash "key3" "value3")
    hash
)
;;=> {"key1" "value1", "key2" "value2", "key3" "value3"}
```

## Using reflections

clojure.reflect reflects Java's classes. The following code gets the signature of methods that start with "get" using reflection:

```
(use 'clojure.reflect)
;;=>nil
(use 'clojure.pprint)
;;=>nil
(->> (clojure.reflect/reflect java.lang.String)
    :members
    (filter #(.startsWith (str (:name %)) "get"))
    (clojure.pprint/pprint))
;;=> {:name getChars,
;;=> :return-type void,
;;=> :declaring-class java.lang.String,
;;=> :parameter-types [int int char<> int],
;;=> :exception-types [],
;;=> :flags #{:public}}
;;=> {:name getBytes,
;;=> :return-type byte<>,
;;=> :declaring-class java.lang.String,
;;=> :parameter-types [],
;;=> :exception-types [],
;;=> :flags #{:public}}
;;=> ....
```

## Extending Java superclasses and implementing Java interfaces

Java provides a superclass and interface mechanism to provide encapsulation and extendibility. Let's have a look at how to extend existing Java superclasses and implement Java interfaces in Clojure.

## Getting ready

In this chapter, we will learn how to define subclasses and how to implement superinterfaces. We will use Jackson and other libraries. So, the :dependencies section in your project.clj as follows:

```
:dependencies [[org.clojure/clojure "1.8.0"]
                [org.clojure/math.numeric-tower "0.0.4"]
                [com.fasterxml.jackson.core/jackson-core
"2.7.0"]
                [com.fasterxml.jackson.core/jackson-databind
"2.7.0"]
                [com.fasterxml.jackson.core/jackson-
annotations "2.7.0"]
                ]
```

## How to do it...

Let's learn how to use proxy to implement Java interfaces.

## Using proxy

proxy generates an anonymous class for superclasses and interfaces. In the previous recipe, we used the java.lang.Thread class. We will implement java.lang.Runnable when we instantiate:

```
(def t
  (Thread.
    (proxy [java.lang.Runnable] []
      (run [] (println "Thread is called ..."))
      )))
;;=> #'chapter05.proxy/t
```

```
(.start t)
;;=> Thread is called ...
;;=> nil
```

proxy can extend superclasses. The following code overrides the default method of the java.util.HashMap:

```
(def m
  (proxy [java.util.HashMap] [0.75 10]
    (put [k v] (println "key=" k "value=" v " is going to be inserted")
      (proxy-super put k v))))
;;=> #'chapter05.proxy/m
(.put m 1 1)
    (doto (java.util.HashMap.)
      (.put 1 1)
    )
;;=> key= 1 value= 1  is going to be inserted
;;=> {{1 1}}
```

The second proxy parameter is the constructor parameter of the HashMap. The proxy-super calls supermethods.

## Using reify for implementing interfaces

We can also implement interfaces using reify. The following code is the reify version of implementing java.lang.Runnable:

```
(def t
  (Thread.
    (reify
      java.lang.Runnable
      (run [this]
        (println "Thread is called ...")
      ))))
;;=> #'chapter05.proxy/t
(.start t)
;;=> Thread is called ...
;;=> nil
```

## How it works...

proxy and reify are similar, but the following differences apply:

- `reify` does not support extending superclasses, but proxy does
- `reify` requires object references for the parameter list, but proxy doesn't
- The performance of `reify` is better than that of proxy

## There's more...

When we are writing Java code, we sometimes define anonymous classes to define methods for specific purposes but not used by others. We can do the same thing in Clojure.

## Defining anonymous classes in Clojure

Inline definition of anonymous classes is a common technique in Java:

```
import java.util.List;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;

public class ComparatorTest {
    public static void main(String [] args) {
        List<Integer> list = Arrays.asList(1, 3, 2, 5, 8);
        Collections.sort(list, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1, o2);
            }
        });
        System.out.println(list);
    }
}
```

We will try to write the equivalent code in Clojure as follows:

```
(defn list-sort [li]
  (java.util.Collections/sort li
    (reify java.util.Comparator
      (compare [this x y]
        (Integer/compare x y)))))
;;=> #'chapter05.proxy/list-sort
(def l1 (java.util.ArrayList. [3 1 5 2 4]))
;;=> #'chapter05.proxy/l1
(clojure.pprint/pprint l1)
;;=> [3 1 5 2 4]
;;=> nil
```

```
(list-sort l1)
;;=> nil
(clojure.pprint/pprint l1)
;;=> [1 2 3 4 5]
;;=> nil
```

## Using annotations

We can make use of Java annotations in Clojure. Example code using Jackson to handle JSON and use Java annotations is as follows:

```
(import [com.fasterxml.jackson.databind ObjectMapper])
;;=> com.fasterxml.jackson.databind.ObjectMapper
(import [com.fasterxml.jackson.annotation JsonCreator
JsonProperty])
;;=> com.fasterxml.jackson.annotation.JsonProperty
(deftype ^{JsonProperty {}} Employee
  [^{:tag String JsonProperty "name"} name
   ^{:tag String JsonProperty "dept"} dept
   ^{:tag long JsonProperty "salary"} salary
  ]
)
;;=> chapter05.proxy.Employee
```

The preceding code defines the Employee class with name, dept, and salary fields. The next code instantiates an Employee object:

```
(def makoto (->Employee "Makoto" "Development" 10000))
;;=> #'chapter05.proxy/makoto
```

We can get a JSON string using writeValueAsString of the ObjectMapper class as follows:

```
(.writeValueAsString (ObjectMapper.) makoto)
;;=> {"name":"Makoto","dept":"Development","salary":10000}
```

## Calling Clojure from Java

In this recipe, we will learn how to define Java classes in Clojure and call them from Java.

## Getting ready

To include Java sources in a Leiningen project, we create the project.clj as follows and restart the REPL:

```
(defproject chapter05 "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :license {:name "Eclipse Public License"
            :url "http://www.eclipse.org/legal/epl-v10.html"}
  :java-source-paths ["src/java"]
  :aot [chapter05.calling-from-java]
  :prep-tasks [
                ["compile" "chapter05.calling-from-java"]
                "javac" "compile" ]
  :main chapter05.TestHello
  :dependencies [[org.clojure/clojure "1.8.0"]
                 ])
```

## How to do it...

We will take a look at how we define Java classes that can be called from Java.

## Defining a simple named class using gen-class

The following code is a very simple code to print string using proxy:

```
(ns chapter05.calling-from-java)
(gen-class
  :name cljbook.chapter05.Hello
  :methods [^:static [hello [String] void]])
;;=> nil
(defn- -hello
  [s]
  (println (str "Hello " s)))
;;=> nil
```

We write the following code to call the hello method in the Hello class written in Clojure:

```
package chapter05;
import cljbook.chapter05.Hello;
public class TestHello {
    public static void main(String[] args) {
        Hello.hello("makoto");
    }
}
```

TestHello.java is located under src/java/chapter05. We will invoke the Clojure code in src/chapter05/calling_from_java.clj.

So, let's go to your terminal and issue a command as follows:

```
$ lein run -m chapter05.TestHello
Compiling 1 source files to /home/makoto/clojure/clojure-packt-book/chapter05/target/classes
Hello makoto
```

## Defining a class with instance methods and constructors in Clojure

Here, we will learn how to define instance methods and constructors using gen-class. Our code defines the Person class in the cljbook.chapter05 package, which has three constructors and two pairs of getters/setters for 22 fields:

```
(gen-class
:name cljbook.chapter05.Person
:state state
:init init
:prefix "-"
:main false
:constructors {
                [] []
                [String] []
                [String String] []}
:methods [
                [setCountry [String] void]
                [getCountry [] String]
                [setName [String] void]
```
