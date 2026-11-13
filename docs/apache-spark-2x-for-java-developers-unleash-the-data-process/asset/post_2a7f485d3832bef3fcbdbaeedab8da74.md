
## What's new in Spark 2.X?

- **Unified DataFrame and Dataset**: The Spark 2.X release has unified both the APIs. Now Dataframe is just a row in Dataset without any data type information implicitly attached.

- **SparkSession**: Prior to Spark 2.X, there were different entry points for different Spark jobs; that is, for Spark SQL we had `sqlContext` and if Hive features were also required then `HiveContext` was the entry point. With Spark 2.X this ambiguity has been removed and now we have one single entry point called `SparkSession`. However, it is to be noted that all the module-specific entry points are still very much around and have not been deprecated yet.

- **Catalog API**: Spark 2.X has introduced the Catalog API for accessing metadata information in Spark SQL. It can be seen as parallel to **Hcatalog** in Hive. It is a great step in unifying the metadata structure around Spark SQL so that the very same metadata can be exposed to non-Spark SQL applications. It is also helpful in debugging the temporary registered table in a Spark SQL session. Metadata of both `sqlContext` and `HiveContext` are available now, as the Catalog API can be accessed by `SparkSession`.

- **Structured streaming**: Structured streaming makes Spark SQL available in streaming job by continuously running the Spark SQL job and aggregating the updated results on a streaming datasets. The Dataframe and Dataset are available for operations in structured streaming along with the windowing function.

- **Whole-stage code generation**: The code generation engine has been modified to generate more performance-oriented code by avoiding virtual function dispatches, transfer of intermediate operation data to memory, and so on.

- **Accumulator API**: A new simpler and more performant Accumulator API has been added to the Spark 2.X release and the older API has been deprecated.

- A native SQL parser that supports both ANSI-SQL as well as Hive SQL has been introduced in the current Spark build.

- Hive-style bucketing support too has been added to the list of supported SQL functions in Spark SQL.
- Subquery support has been added in Spark SQL and supports other variations of the clause such as NOT IN, IN, EXISTS, and so on.
- Native CSV data source, based on the databricks implementation has been incorporated in Spark.
- The new spark.ml package which is based on Dataframe has been introduced with an objective to deprecate spark.mllib once the newly introduced package matures enough in features to replace the old package.
- Machine learning pipelines and models can now be persisted across all languages supported by Spark.

## References

- http://www.evl.uic.edu/cavern/rg/20040525_renambot/Viz/parallel_volviz/paging_outofc_viz97.pdf
- http://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf
- http://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf
- Mastering Apache Solr: A practical guide to get to grips with Apache Solr by Mr. Mathieu Nayrolles [Page 6]
- https://web.archive.org/web/200510209085121/http://weblogs.java.net/blog/tomwhite/archive/2005/09/mapreduce.html
- Nutch: A Flexible and Scalable Open-Source Web Search Engine, http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.105.5978&rep=rep1&type=pdf
- https://developer.yahoo.com/blogs/hadoop/yahoo-launches-world-largest-hadoop-production-application-398.html
- http://data-informed.com/doug-cutting-reflects-on-hadoops-impact-future/

## Summary

In this chapter, you learned about the history of Bigdata and its dimensions and basic concepts of Hadoop and Spark. After that, you were introduced to RDD, and you learned the basic concepts of the Spark RDD model. Then, you learned about the various components of the Spark Ecosystem and the newer features of Spark 2.x.

As this book focuses on implementing Spark Applications in Java, in the next chapter, we will refresh the concepts of core Java and focus on the newer feature of Java 8, which will be leveraged while developing Spark Applications.

## Revisiting Java

This chapter is added as the refresher course of Java. In this chapter, we will discuss some concepts of Java that will be useful while creating applications in Apache Spark.

This book assumes that the reader is comfortable with the basics of Java. We will discuss useful Java concepts and mainly focus on *what is new in Java 8?* More importantly, we will touch upon on topics such as:

- Generics
- Interfaces
- Lambda expressions
- Streams

## Why use Java for Spark?

With the rise in multi-core CPUs, Java could not keep up with the change in its design to utilize that extra power available to its disposal because of the complexity surrounding concurrency and immutability. We will discuss this in detail, later. First let's understand the importance and usability of Java in the Hadoop ecosystem. As MapReduce was gaining popularity, Google introduced a framework called **Flume Java** that helped in pipelining multiple MapReduce jobs. Flume Java consists of immutable parallel collections capable of performing lazily evaluated optimized chained operations. That might sound eerily similar to what Apache Spark does, but then even before Apache Spark and Java Flume, there was Cascading, which built an abstraction over MapReduce to simplify the way MapReduce tasks are developed, tested, and run. All these frameworks were majorly a Java implementation to simplify MapReduce pipelines among other things.

These abstractions were simple in fact, **Apache Crunch**, Apache's implementation of Flume Java, was so concise that a word count problem could be written in four-five lines of Java code. That is huge!

Nevertheless these Java implementations could never do away with the heavy I/O bound operations. And it was just then that Apache Spark was born, primarily written in Scala, which not only addressed the limitations of MapReduce, but also the verbosity of Java while developing a MapReduce Job without using any abstraction framework. In their paper, *Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing*, the authors state that:

> "We chose Scala due to its combination of conciseness (which is convenient for interactive use) and efficiency (due to static typing). However, nothing about the RDD abstraction requires a functional language."

Eventually the Java community realized that with time they would have to support functional programming to harness the capacity of cluster computing as vertical scaling of enterprise hardware was not the solution for applications being developed for the future. Java started releasing features around concurrency. Java 7 introduced the fork join framework to harness the capabilities of modern hardware; it also introduced an instruction set `invokedynamic`, which enabled method calls to be made at runtime. This single instruction set, change allowed dynamically typed language on the JVM. Java 8 beautifully utilized the concept of late binding in `invokedynamic` and developed entire Lambda expressions around it. It also released the Stream API, which can be thought of as a collection framework for functional programming in Java without actually storing the elements. Other notable changes in Java 8 are:

- Functional interfaces
- Default and static methods in interfaces
- New date and time API
- Scripting support

Apache Spark has added support for Java 8 and hence the difference in writing a code in Scala and Java is blurring with each release. The learning curve also matches its potential as one is left to learn only the Apache Spark API and not Scala as well. Another highlight of Apache Spark is Spark-SQL, which can come in handy if interactive and dynamic coding is required.

## Generics

Generics were introduced in Java 1.5. Generics help the user to create the general purpose code that has abstract type in its definition. That abstract type can be replaced with any concrete type in the implementation.

For example, the list interface or its implementations, such as ArrayList, LinkedList and so on, are defined with generic type. Users can provide the concrete type such as Integer, Long, or String while implementing the list:

```
List<Integer> list1 = new ArrayList<Integer>();
List<String> list2 = new ArrayList<String>();
```

Here, list1 is the list of integers and list2 is the list of strings. With Java 7, the compiler can infer the type. So the preceding code can also be written as follows:

```
List<Integer> list1 = new ArrayList<>();
List<String> list2 = new ArrayList<>();
```

Another huge benefit of generic type is that it brings compile-time safety. Let's create a list without the use of generics:

```
List list = new ArrayList<>();
list.add(1);
list.add(2);
list.add("hello");
list.add("there");
```

Here, you can see that the list contains both integers and strings. If you retrieve an element of it, it will be of an object type by default:

```
Object object = list.get(0);
```

So the user has to specifically cast it to its correct type:

```
Integer object = (Integer)list.get(0);
```

This makes the code error prone. Because if casting is done correctly, it will throw ClassCastException at runtime. Let's create the same list with the use of
