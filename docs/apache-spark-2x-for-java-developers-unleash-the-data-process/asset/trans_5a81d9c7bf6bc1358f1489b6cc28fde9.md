

## 驱动程序

以下是从 Spark 作业主函数中实例化和调用累加器的代码：

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

## 总结

本章介绍了管辖类 RDD 分区运行的各种主题，随后使用了高级转换和操作来实现特定需求。我们还探讨了跨执行器节点共享变量的局限性，以及如何使用广播变量和累加器来实现这一目标。

下一章将介绍 Spark SQL 及相关概念，如 DataFrame、Dataset、UDF 等。我们还将讨论 `SQLContext` 和新引入的 `SparkSession`，以及其引入如何简化处理 Hive 元数据存储的整个过程。

## 使用 Spark SQL

本章将介绍 Spark SQL 及相关概念，如 DataFrame 和 Dataset。将从 Apache Spark 的角度讨论 Schema 和高级 SQL 函数；编写自定义 **用户定义函数（UDF）** 以及处理各种数据源也将被涉及。

本章使用 Java API 创建 `sqlContext/SparkSession`，并从 Java RDD 为原始数据（如 CSV）和结构化数据（如 JSON）实现 DataFrame/Dataset。

## SQLContext 和 HiveContext

在 Spark 2.0 之前，SparkContext 曾是 Spark 应用程序的入口点，而 SQLContext 和 HiveContext 曾是运行 Spark SQL 的入口点。HiveContext 是 SQLContext 的超集。需要创建 SQLContext 才能在 RDD 上运行 Spark SQL。

SQLContext 提供对各种数据源的连接。可以从这些数据源读取数据，并执行 Spark SQL 以根据需要转换数据。可以使用 SparkContext 如下创建它：

```java
JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
SQLContext sqlContext = new SQLContext(javaSparkContext);
```

SQLContext 在 SparkContext 之上创建了一个包装器，并提供 SQL 功能和用于处理结构化数据的函数。它带有基本级别的 SQL 函数。

作为 SQLContext 的超集，HiveContext 提供了更多功能。HiveContext 允许你使用 Hive QL 解析器编写查询，这意味着可以使用 HiveContext 访问所有 Hive 函数。例如，可以使用 javaSparkContext 的 HiveContext 执行窗口函数。与此同时，它还提供对 Hive UDF 的访问。与 SQLContext 类似，可以使用 SparkContext 如下创建 HiveContext：

```java
JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
HiveContext hiveContext = new HiveContext(javaSparkContext);
```

HiveContext 运行不需要 Hive。但是，如果需要，可以使用 HiveContext 查询 Hive 表。访问 Hive 表需要 hive-site.xml。此外，所有使用 SQLContext 可访问的数据源，也可以使用 HiveContext 访问。

HiveContext 需要包含所有 Hive 依赖项。因此，如果只需要基本 SQL 函数，则 SQLContext 就足够了。但是，建议使用 HiveContext，因为它提供了广泛的功能。

在 Spark 2.x 中，这两个 API 均可用。但是，根据文档，它们已被弃用。Spark 2.0 为所有 Spark 应用程序引入了一个名为 SparkSession 的新入口点。

```
JavaSparkContext SparkSession
```

在第 5 章“使用数据和存储”中，我们使用了 SparkSession 连接到 Cassandra 并读取各种格式的数据。在本节中，我们将详细讨论 SparkSession。

The SparkSession was introduced in Spark 2.0 as the single entry point for all Spark applications. Prior to Spark 2.0, sparkContext used to be the entry point for Spark applications: SQLContext and HiveContext were used for Spark SQL, StreamingContext was used for Spark streaming applications, and so on.

We will explain Spark streaming in the next chapter.

In Spark 2.x, SparkSession can be used as the entry point for every Spark application. It combines all the functions of SQLContext, HiveContext, and StreamingContext. The SparkSession internally requires a SparkContext for computation.

Another key difference in SparkSession is that there can be multiple SparkSession instances in a single Spark application, which was not possible with SparkContext.

SparkSession 于 Spark 2.0 引入，作为所有 Spark 应用程序的单一入口点。在 Spark 2.0 之前，sparkContext 曾是 Spark 应用程序的入口点：SQLContext 和 HiveContext 用于 Spark SQL，StreamingContext 用于 Spark 流式处理应用程序，等等。

我们将在下一章解释 Spark 流式处理。

在 Spark 2.x 中，SparkSession 可以用作每个 Spark 应用程序的入口点。它结合了 SQLContext、HiveContext 和 StreamingContext 的所有功能。SparkSession 在内部需要一个 SparkContext 来进行计算。

SparkSession 的另一个关键区别是，在单个 Spark 应用程序中可以存在多个 SparkSession 实例，而使用 SparkContext 时则不可能。

## 初始化 SparkSession

让我们创建一个 SparkSession 对象。SparkSession 遵循建造者设计模式，因此我们可以按如下方式初始化 SparkSession：

```java
SparkSession sparkSession = SparkSession.builder()
    .master("local")
    .appName("Spark Session Example")
    .getOrCreate();
```

你一定已经注意到，我们没有为初始化 SparkSession 创建任何 SparkContext 或 SparkConf 对象。SparkConf 和 SparkContext 已封装在 sparkSession 中。使用 SparkSession 时不需要显式初始化这些对象。

可以使用 SparkSession 对象按如下方式访问 SparkConf 和 SparkContext：

```java
SparkContext sparkContext = sparkSession.sparkContext();
SparkConf conf = sparkSession.sparkContext().getConf();
```

可以使用 sparkSession 运行 Spark SQL。但是，默认情况下未启用 Hive SQL 支持。要启用它，可以按如下方式初始化 SparkSession：

```java
SparkSession sparkSession = SparkSession.builder()
    .master("local")
    .appName("Spark Session Example")
    .enableHiveSupport()
    .getOrCreate();
```

如第 3 章“让我们 Spark”中所述，可以在初始化 sparkContext 之前在 SparkConf 对象中设置运行时配置。由于创建 SparkSession 不需要显式创建 SparkConf，因此可以在 SparkSession 中提供 Spark 应用程序的配置参数，如下所示：

```java
SparkSession sparkSession = SparkSession.builder()
    .master("local")
    .appName("Spark Session Example")
    .config("spark.driver.memory", "2G")
    .getOrCreate();
```

初始化 Spark 对象后，还可以按如下方式更改运行时配置：

```java
sparkSession.conf().set("spark.driver.memory", "3G");
```

## 使用 SparkSession 读取 CSV

在第 5 章“使用数据和存储”中，我们使用 sparkSession 以 Java RDD 的形式读取了 CSV。不过，这次我们将以 Dataset 的形式读取 CSV。假设你有一个包含以下内容的 CSV：

```
emp_id,emp_name,emp_dept
1,Foo,Engineering
2,Bar,Admin
```

可以使用 SparkSession 按如下方式读取此 CSV 文件：

```java
Dataset<Row> csv = sparkSession.read().format("csv").option("header", "true").load("");
```

类似于 RDD 上的 collect() 函数，Dataset 提供 show() 函数，可用于读取 Dataset 的内容：

```java
csv.show();
```

执行此函数将显示带有标题的 CSV 文件内容，看起来类似于关系表。Dataset 的内容可以使用 Spark SQL 进行转换/过滤，这将在后续章节中讨论。

