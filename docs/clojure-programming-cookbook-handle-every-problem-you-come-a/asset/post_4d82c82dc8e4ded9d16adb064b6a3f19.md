
```
(pmap
#(async/>!! in-chan %)
(line-seq in)))
```

Suppose the test_0.xml file content is as follows:

```
<friend age="23">Friend1</friend>
<friend age="23">Friend4</friend>
...
```

Then the output of the reading with core-async simply goes through a one-by-one flow, as seen in the following diagram:

![](img/d7b631352a782ee50160cf8eb995152f_284_0.png)

## There's more...

The same parsing and handling techniques of data could be also applied to reducers:

```
(require '[clojure.core.reducers :as r])
(use 'clojure.data.xml)
(use '[clojure.java.io :only [writer reader input-stream output-stream]])
```

```
(with-open [in (reader (input-stream "test_1.xml"))]
(into []
(r/filter
```

```
(fn[x]
  (= "Friend1"
  (first (:content (parse
  (java.io.StringReader. x))))))
(line-seq in)))
```

This has the drawback of loading more data in memory than needed, but processing is easier to read and concise.

If multiple actions are to be taken while handling the data, transducers could also be added to the mix. We leave that to the curiosity of the reader!

## Using Apache Camel to connect everything

Apache Camel is a rule-based routing and mediation engine that provides the Enterprise Integration Patterns using an API or declarative Java domain-specific language to configure routing and mediation rules.

## Getting ready

In this recipe, we will use the Java Camel library. So, you need to add the org.apache.camel/camel-core to your project.clj:

```
(defproject chapter04 "0.1.0"
:dependencies [[org.clojure/clojure "1.8.0"]
                [org.apache.camel/camel-core "2.16.1"]]
```

## How to do it...

Here, we will learn basic uses of Camel from Clojure.

## Importing Camel classes

The following code imports the Camel classes that we will use:

```
(ns chapter04.camel
  (:import [org.apache.camel.impl DefaultCamelContext]
           [org.apache.camel.builder RouteBuilder]
           [org.apache.camel.main Main]
           [org.apache.camel Exchange]
           [org.apache.camel Processor]))
```

## Your first ride on Camel with Clojure

Let's write a simple Camel code that moves files from one directory to the other. The following code is to move a file from /tmp/in to /tmp/out:

```
(def ctx
```

```
(let [context (DefaultCamelContext.)]
  (.addRoutes context (proxy [RouteBuilder] []
    (configure [] (.. this
      (from
"file:/tmp/in?noop=false")
      (to
"file:/tmp/out")))))
  context
  ))
#'chapter04.camel/ctx
```

Start the context:

```
(.start ctx)
;;=> nil
```

Then create a file under the /tmp/in:

```
$ echo hello Makoto ! > /tmp/in/makoto.txt
```

The Camel code checks under the /tmp/in and moves to /tmp/out:

![](img/d7b631352a782ee50160cf8eb995152f_287_0.png)

Let's stop Camel as follows:

```
(.stop ctx)
;;=> nil
```

## Writing more Clojure idiomatic code with Camel

The preceding code is simpler compare to the following Java code:

```
public class FileToFile {
    public static void main(String args[]) throws Exception {
        CamelContext context = new DefaultCamelContext();
```

```
context.addRoutes(new RouteBuilder() {
    public void configure() {
        from("file:/tmp/in?noop=false")
            .to("file:/tmp/out");
    }
});
context.start();
}
```

Using macro makes application code simpler. We will define a helper function that implements org.apache.camel.builder RouteBuilder and a macro:

```
(defn create-builder [builder]
  (proxy [RouteBuilder] [] (configure [] (builder this))))
;;=> #'chapter04.camel/create-builder
(defmacro add-routes [ctx & body]
  `(.addRoutes ~ctx (create-builder (fn [~'this] (.. ~'this ~@body)))))
;;=>#'chapter04.camel/add-routes
```

Using macro, code is much simpler, and the route definition is easy to understand:

```
(defn file-to-file []
  (let [context (DefaultCamelContext.)]
    (add-routes context
      (from "file:/tmp/in?noop=false")
      (to "file:/tmp/out")
      )
    context
  ))
;;=> #'chapter04.camel/file-to-file
```

We can start and stop the router using .start and .stop, respectively:

```
(def ctx (file-to-file))
;;=> #'chapter04.camel/ctx
(.start ctx)
;;=> nil
(.stop ctx)
;;=> nil
```

## How it works...

The preceding code uses file endpoints. We will show you major parameters for the endpoint as follows:

| Name | Default value | Description |
|---|---|---|
| noop | false | If true, the file is not moved or deleted in any way. This option is good for read only data or for ETL type requirements. |
| append | true | When writing, do we append to the end of the file or replace it? |
| consumer.delay | 500 | Milliseconds before the next poll of the file/directory. |

## There's more...

Let's define a very simple processor that transforms messages between the file endpoints. To write a processor, we have to implement a process method in the org.apache.camel Processor class. We will use the Clojure proxy function to implement this method. Our simple processor echoes an inbound string and adds a string "Hello " on it and sends it to the outbound:

```
(defn get-payload-as-string [ex]
  (-> ex .getIn (.getBody (class ""))))
;;=> #'chapter04.camel/get-payload-as-string
(def simple-processor
  (proxy [Processor] []
    (process [exchange]
      (let [payload
            (get-payload-as-string exchange)]
        (println payload)
        (-> exchange .getOut (.setBody (str "Hello " payload)))))))
#'chapter04.camel/processor
```

So, let's define a new context using the processor:

```
(defn file-to-file-with-processor []
```

```
(let [context (DefaultCamelContext.)]
  (add-routes context
    (from "file:/tmp/in?noop=false")
    (process simple-processor)
    (to "file:/tmp/out"))
  context))
;;=> #'chapter04.camel/processor
```

The following code runs Camel:

```
(def ctx (file-to-file-with-processor))
;;=> #'chapter04.camel/ctx
(.start ctx)
;;=> nil
```

Let's run the following commands:

```
$ echo Makoto > /tmp/in/makoto.txt
$ echo Nico > /tmp/in/nico.txt
```

Echo strings are displayed on your console, and file contents are converted, as shown in the following screenshot:

![](img/d7b631352a782ee50160cf8eb995152f_290_0.png)

## See also

You can have a look at the Apache Camel home page at http://camel.apache.org/ .

# Chapter 5. Working with Other Languages

In this chapter, we will cover the following topics:

- Calling Java methods and accessing Java objects from Clojure
- Extending Java superclasses and implementing Java interfaces
- Calling Clojure from Java
- Calling Scala from Clojure
- ClojureCLR
- ClojureScript

## Introduction

Clojure runs on the **Java Virtual Machine (JVM)**, and the Clojure language provides full access to Java. Java has huge assets and we can use all of them from Clojure. This is one of the great advantages of Clojure, which is also callable from Java.

In this chapter, you will see how to instantiate objects from classes and how to access Java objects, including methods and fields, and also learn ways to inherit superclasses and ways to implement interfaces.

You will also learn how to use Clojure on Microsoft's .Net and JavaScript, using ClojureCLR and ClojureScript. This chapter will introduce to you how to set up ClojureCLR and ClojureScript environments and how to smoothly write code on them.

## Calling Java methods and accessing Java objects from Clojure

Though the Clojure language looks quite different from Java, Clojure provides full access to Java. This enables Clojure to make use of huge Java assets very efficiently. In general, calling Java assets from Clojure is much simpler and results in less code than calling them from Java. Another big advantage is REPL, which makes programming fun and faster.

## Getting ready

In this recipe, we don't use an external library, so the only necessary thing is to start REPL to run code.

## How to do it...

We will learn how to instantiate objects, call instance and class methods, access fields and inner classes, and reference classes. Then, we will learn how to manipulate Java arrays.

## Instantiating objects

To instantiate an object from a class, use the following syntax:

```
(ClassName. arg1 arg2 ...)
```

For instance, to instantiate a new String object the syntax is as follows:

```
(String. "abc")
;;=> "abc"
(Integer. 1)
;;=> 1
```

## Calling instance methods
