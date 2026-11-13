

```java
accList.add(Integer.parseInt(arg));
}
@Override
public AccumulatorV2<String, CopyOnWriteArrayList<Integer>> copy() {
    return new ListAccumulator(value());
}

@Override
public boolean isZero() {
    return accList.size() == 0 ? true : false;
}

@Override
public void merge(AccumulatorV2<String, CopyOnWriteArrayList<Integer>>
other)
{
    add(other.value());
}

private void add(CopyOnWriteArrayList<Integer> value) {
    value().addAll(value);
}

@Override
public void reset() {
    accList = new CopyOnWriteArrayList<Integer>();
}

@Override
public CopyOnWriteArrayList<Integer> value() {
    return accList;
}
}
```

## Driver program

Following is the code to instantiate and call the accumulator from the Spark Job's main function:

```java
ListAccumulator listAccumulator = new ListAccumulator();
sparkContext.sc().register(listAccumulator, "ListAccumulator");
textFile.foreach(new VoidFunction<String>() {
    @Override
    public void call(String line) throws Exception {
        if (line.contains("Exception")) {
            listAccumulator.add("1");
            System.out.println("The intermediate value in loop " + listAccumulator.value());
        }
    }
});
System.out.println("The final value of Accumulator : " + listAccumulator.value());
```

## Summary

This chapter covered various topics that govern the functioning of RDD-like partitioning and then used advanced transformations and actions to achieve specific requirements. We also looked at the limitations of sharing variables across executor nodes and how it can be achieved using broadcast variables and accumulators.

The next chapter introduces Spark SQL and related concepts like dataframe, dataset, UDF and so on. We'll also discuss `SQLContext` and the newly introduced `SparkSession` and how its introduction has simplified the whole process of dealing with the Hive metastore.

## Working with Spark SQL

This chapter will introduce Spark SQL and related concepts, like dataframe and dataset. Schema and advanced SQL functions will be discussed from the Apache Spark perspective; and writing custom **user-defined function (UDF)** and working with various data sources will also be touched upon.

This chapter uses Java APIs to create `sqlContext/SparkSession` and implement dataframes/datasets from Java RDD for raw data, such as CSV, and structured data, such as JSON.

## SQLContext and HiveContext

Prior to Spark 2.0, SparkContext used to be the entry point for Spark applications, an SQLContext and HiveContext used to be the entry points to run Spark SQL. HiveContext is the superset of SQLContext. The SQLContext needs to be created to run Spark SQL on the RDD.

The SQLContext provides connectivity to various data sources. Data can be read from those data sources and Spark SQL can be executed to transform the data as per the requirement. It can be created using SparkContext as follows:

```java
JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
SQLContext sqlContext = new SQLContext(javaSparkContext);
```

The SQLContext creates a wrapper over SparkContext and provides SQL functionality and functions to work with structured data. It comes with the basic level of SQL functions.

The HiveContext, being a superset of SQLContext, provides a lot more functions. The HiveContext lets you write queries using Hive QL Parser, which means all of the Hive functions can be accessed using HiveContext. For example, windowing functions can be executed using javaSparkContext HiveContext. Along with that it provides access to Hive UDFs. Similarly to SQLContext, HiveContext can be created using SparkContext as follows:

```java
JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
HiveContext hiveContext = new HiveContext(javaSparkContext);
```

The HiveContext does not require Hive to run. However, if needed, Hive tables can be queried using HiveContext. hive-site.xml is needed to access Hive tables. Also, all the data sources, which are accessible using SQLContext, can also be accessed using HiveContext.

HiveContext requires all hive dependencies to be included. So, if only basic SQL functions are required, then SQLContext will suffice. However, it's recommended to use HiveContext as it provides a wide range of functions.

In Spark 2.x both of these APIs are available. However, as per the documentation, they are deprecated. Spark 2.0 introduced a new entry point for all Spark applications called SparkSession.

```
JavaSparkContext SparkSession
```

In Chapter 5, Working with Data and Storage, we used SparkSession to connect to Cassandra and to read data in various formats. In this section, we will discuss SparkSession in detail.

The SparkSession was introduced in Spark 2.0 as the single entry point for all Spark applications. Prior to Spark 2.0, sparkContext used to be the entry point for Spark applications: SQLContext and HiveContext were used for Spark SQL, StreamingContext was used for Spark streaming applications, and so on.

We will explain Spark streaming in the next chapter.

In Spark 2.x, SparkSession can be used as the entry point for every Spark application. It combines all the functions of SQLContext, HiveContext, and StreamingContext. The SparkSession internally requires a SparkContext for computation.

Another key difference in SparkSession is that there can be multiple SparkSession instances in a single Spark application, which was not possible with SparkContext.

## Initializing SparkSession

Let's create a SparkSession object. SparkSession follows the builder design pattern, therefore we can initialize SparkSession in the following way:

```java
SparkSession sparkSession = SparkSession.builder()
    .master("local")
    .appName("Spark Session Example")
    .getOrCreate();
```

You must have noticed that we have not created any SparkContext or SparkConf objects for initializing SparkSession. The SparkConf and SparkContext are encapsulated in sparkSession. No explicit initialization of these objects is required with SparkSession.

The SparkConf and SparkContext can be accessed using the SparkSession object as follows:

```java
SparkContext sparkContext = sparkSession.sparkContext();
SparkConf conf = sparkSession.sparkContext().getConf();
```

You can run Spark SQL using sparkSession. However, Hive SQL support is not enabled by default. To enable it SparkSession can be initialized as follows:

```java
SparkSession sparkSession = SparkSession.builder()
    .master("local")
    .appName("Spark Session Example")
    .enableHiveSupport()
    .getOrCreate();
```

As described in Chapter 3, Let's Spark, runtime configurations can be set in the SparkConf object before initializing sparkContext. Since we do not need to create SparkConf explicitly for creating SparkSession, there are configurational parameters for the Spark application that can be provided in SparkSession as follows:

```java
SparkSession sparkSession = SparkSession.builder()
    .master("local")
    .appName("Spark Session Example")
    .config("spark.driver.memory", "2G")
    .getOrCreate();
```

After initializing a Spark object, runtime configurations can be altered as well as follows:

```java
sparkSession.conf().set("spark.driver.memory", "3G");
```

## Reading CSV using SparkSession

In Chapter 5, Working with Data and Storage, we read CSV using sparkSession in the form of a Java RDD. However, this time we will read the CSV in the form of a dataset. Consider, you have a CSV with the following content:

```
emp_id,emp_name,emp_dept
1,Foo,Engineering
2,Bar,Admin
```

The SparkSession can be used to read this CSV file as follows:

```java
Dataset<Row> csv = sparkSession.read().format("csv").option("header", "true").load("");
```

Similarly to the collect() function on RDD, a dataset provides the show() function, which can be used to read the content of the dataset:

```java
csv.show();
```

Executing this function will show the content of the CSV files along with the headers which seems similar to a relational table. The content of a dataset can be transformed/filtered using Spark SQL which will be discussed in the next sections.

