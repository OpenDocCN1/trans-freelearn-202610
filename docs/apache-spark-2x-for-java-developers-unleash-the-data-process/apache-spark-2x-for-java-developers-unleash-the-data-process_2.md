## 使用数据和存储

在上一章中，我们讨论了可以在 Spark 上执行的各种转换和动作。为了在 Spark 中加载数据进行处理，它需要与外部存储系统交互。在本章中，我们将学习如何从不同的存储系统向 Spark 读取/存储数据。我们还将讨论帮助在 Spark 中处理各种结构化/非结构化数据的库。

## 与外部存储系统的交互

众所周知，Spark 是一个处理引擎，可以帮助处理海量数据；但是，要处理数据，必须先从外部系统读取数据。在本节中，我们将学习如何从不同的存储系统向 Spark 存储/读取数据。

我们将从本地文件系统开始，然后实现 Spark 与大数据世界中一些常用存储系统的集成。

## 与本地文件系统的交互

在 Spark 中从本地文件系统读取数据非常简单直接。我们将通过示例来讨论，如下所示：

先解决首要问题。首先，创建（或复用）上一章中描述的 Maven 项目，并为我们的应用程序创建一个 Java 类（带有 main 方法）。我们将从创建 JavaSparkContext 开始：

```
SparkConf conf = new SparkConf().setMaster("local").setAppName("Local File system Ex:
JavaSparkContext jsc = new JavaSparkContext(conf);
```

要在 Spark 中读取文本文件，SparkContext 提供了 textFile 方法。该方法的签名如下：

```
textFile(String path)
```

此方法可用于从本地文件系统读取文件，如下所示：

```
JavaRDD<String> localFile = jsc.textFile("absolutepathOfTheFileOnLocalFileSystem");
```

如前所示，文件将作为字符串 RDD 读取。现在，我们可以对文件数据运行转换和/或动作。例如，可以在此数据上执行 wordcount，如下所示：

```
JavaPairRDD<String, Integer> wordCountLocalFile = localFile.flatMap(x -> Arrays.asLi
.mapToPair(x -> new Tuple2<String, Integer>((String) x, 1))
.reduceByKey((x, y) -> x + y);
wordCountLocalFile.collect();
```

![](img/a77bd9f099223ea139d6d612f2b90712_228_0.png)

如我们已知，Spark 操作的 DAG 只有在执行动作时才会开始执行。在此，从本地文件系统读取文件也是 DAG 的一部分。因此，只有在执行动作时（即前面示例中的 collect() 操作），文件读取操作才会执行。

textFile 方法可用于读取多个文件，如下所示：

- 读取多个文件：
  - `jsc.textFile("absolutepathOfTheFile1,absolutepathOfTheFile2");`

- 读取目录中的所有文件：
  - `jsc.textFile("absolutepathOfTheDir/*");`

- 读取多个目录中的所有文件：
  - `jsc.textFile("absolutepathOfTheDir1/*,absolutepathOfTheDir2/*");`

textFile 方法还提供一个重载，允许用户指定最小分区数：

```
textFile(String path, int minPartitions)
```

minPartitions 参数有助于计算文件被分割成多少个分片。

Spark 的 textFile 方法在内部使用 Hadoop 的 FileInputFormat[1] 来计算输入分片。计算输入分片所用的公式如下：

```
Math.max(minSize, Math.min(goalSize, blockSize));
```

其中 minSize = mapred.min.split.size 或 mapreduce.input.fileinputformat.split.minsize：

```
goalSize = totalSize / (numSplits == 0 ? 1 : numSplits); (total size is total size ...
```

blockSize 是文件的块大小，在本地文件系统中被认为是 32 MB（hadoop fs.local.block.size），在 HDFS 中则是 64 MB/128 MB（dfs.blocksize）。

用于计算 goalSize 的 numSplits 值就是 minPartitions 参数的值。

> 我们将在第 7 章“Spark 编程模型 – 高级”中进一步学习 RDD 分区。

SparkContext 提供了另一种读取文件的方法：

```
wholeTextFiles(String path)
wholeTextFiles(String path, int minPartitions)
```

该方法返回一个键值对 RDD，键为文件名，值为文件内容：

```
JavaPairRDD<String, String> localFile = jsc.wholeTextFiles("absolutepathOfTheFile")
```

与 textFile 方法类似，可以使用 wholeTextFiles 方法读取多个文件，如下所示：

- 读取多个文件
  - `jsc.wholeTextFiles("absolutepathOfTheFile1,absolutepathOfTheFile2");`

- 读取目录中的所有文件
  - `jsc.wholeTextFiles("absolutepathOfTheDir/*");`

- 读取多个目录中的所有文件
  - `jsc.wholeTextFiles("absolutepathOfTheDir1/*,absolutepathOfTheDir2/**");`

要将输出保存回本地文件系统，可以使用 saveAsTextFile() 方法，如下所示：

```
wordCountLocalFile.saveAsTextFile("absolutePathOfTheOutputDir");
```

> 输出路径不应是已存在的路径，否则将抛出以下异常：
> Exception in thread "main"
> org.apache.hadoop.mapred.FileAlreadyExistsException





## 与 Amazon S3 交互

简单存储服务，也就是 S3，是由 Amazon Web Services (AWS) 提供的一种可自动扩展的存储平台。它是一种广泛用于存储数据的服务。一旦连接到互联网，用户可以随时从 S3 存储和检索任何量的数据。作为互联网上最便宜的存储服务之一，它被广泛用于存储海量数据。由于它是 AWS 服务，用户无需担心其可用性和管理问题。

> 如果您未使用过 S3，建议参阅 AWS S3 文档（http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html）以获取更多信息。

以下文件系统方案可用于在 Spark 中访问 S3 数据：

- S3 (URL: s3://)：此方案绑定到 Hadoop 文件系统。换句话说，此方案是由 S3 支持的 HDFS 的 Hadoop 实现。因此，使用此方案时，文件将实际以块形式存储，由于以块形式存储，除 Hadoop 之外的工具无法将其作为常规文件读取。此方案现已弃用。

> 由于此方案已弃用，我们未在代码示例中考虑它。

- S3N (URI: s3n://)：S3N（又称 S3 Native）是用以常规 S3 对象形式读取/写入文件的方案。使用此方案，Hadoop 写入的文件可被其他工具访问，反之亦然。它需要 jets3t 库才能工作。

使用此方案时，存在 5GB 的上限，即无法使用此方案读取大于 5GB 的文件。尽管此方案被广泛使用，但不再考虑对其进行新增增强。

- S3A (URI: s3a://)：这是最新的文件系统方案。此方案已移除 S3N 的上限限制，并提供各种性能改进。此方案利用 Amazon 的库来访问 S3 对象。S3A 也与 S3N 向后兼容。

根据 Amazon 文档，此方案推荐用于任何新开发。

要从 S3 读取数据，需要以下 AWS 凭证：

- AWS 访问密钥 ID
- AWS 秘密访问密钥

用户可以使用以下任一方式配置 AWS 凭证：

- 凭证可以在 Spark 程序中内联设置，如下所示：
  - **使用 S3N**

```
jsc.hadoopConfiguration().set("fs.s3n.awsAccessKeyId", "accessKeyID");
jsc.hadoopConfiguration().set("fs.s3n.awsSecretAccessKey", "secretAccessKey");
```

  - **使用 S3A**

```
jsc.hadoopConfiguration().set("fs.s3a.awsAccessKeyId", "accessKeyID");
jsc.hadoopConfiguration().set("fs.s3a.awsSecretAccessKey", "secretAccessKey");
```

也可以使用属性文件读取以避免安全问题。这些凭证可以使用环境变量设置，如下所示：

```
export AWS_ACCESS_KEY_ID=accessKeyIDValue
export AWS_SECRET_ACCESS_KEY=securityAccessKeyValue
```

另外，要使用 S3A 文件系统，需要将 hadoop-aws 库添加到类路径中。以下是用于此目的的 Maven 依赖项模板：

```
<dependency>
<groupId>org.apache.hadoop</groupId>
<artifactId>hadoop-aws</artifactId>
<version>2.7.1</version>
</dependency>
```

S3 方案可在 Spark 中按如下方式提供，用于从 S3 读取数据：

- 使用 S3N:

```
JavaRDD<String> s3File = jsc.textFile("s3n://" + "bucket-name" + "/" + "filename");
```

- 使用 S3A:

```
JavaRDD<String> s3File = jsc.textFile("s3a://" + "bucket-name" + "/" + "filename");
```

创建 RDD 后，可以对数据执行进一步的操作（转换/动作）以获得所需结果。例如，可以在其上执行以下转换以生成单词计数：

```
JavaPairRDD<String, Integer> s3WordCount = s3File.flatMap(x -> Arrays.asList(x.split(" ")))
    .mapToPair(x -> new Tuple2<String, Integer>((String) x, 1))
    .reduceByKey((x, y) -> x + y);
```

可按如下方式使用 textfile 方法从 HDFS 读取多个文件：

- 要读取多个文件：
  - 使用 S3N

```
jsc.textFile("s3n://" + "bucket-name" + "/" + "path-of-file1", "s3n://" + "bucket-name" + "/" + "path-of-file2");
```

  - 使用 S3A

```
jsc.textFile("s3a://" + "bucket-name" + "/" + "path-of-file1", "s3a://" + "bucket-name" + "/" + "path-of-file2");
```

- 要读取目录中的所有文件：
  - 使用 S3N

```
jsc.textFile("s3n://" + "bucket-name" + "/" + "path-of-dir/*");
```

  - 使用 S3A

```
jsc.textFile("s3a://" + "bucket-name" + "/" + "path-of-dir/*");
```

- 要读取多个目录中的所有文件
  - 使用 S3N

```
jsc.textFile("s3n://" + "bucket-name" + "/" + "path-of-dir1/*" + "," + "s3n://" + "bucket-name" + "/" + "path-of-dir2/*");
```

  - 使用 S3A

```
jsc.textFile("s3a://" + "bucket-name" + "/" + "path-of-dir1/*" + "," + "s3a://" + "bucket-name" + "/" + "path-of-dir2/*");
```

- 要将输出保存回 S3，可按如下方式使用相应方案：
  - 使用 S3N

```
s3WordCount.saveAsTextFile("s3n://" + "bucket-name" + "/" + "outputDirPath");
```

  - 使用 S3A

```
s3WordCount.saveAsTextFile("s3a://" + "bucket-name" + "/" + "outputDirPath");
```

## 与 HDFS 交互

Hadoop 分布式文件系统 (HDFS) 广泛用于存储海量数据。它是大数据世界中最流行的存储系统。其可扩展性、高可用性和容错性使其成为存储大块数据的理想文件系统。

我们已在第 1 章《Spark 简介》中解释过 HDFS 的一些基础知识。请参阅该章节以进一步了解其工作原理。

在 HDFS 上读取或写入数据时，客户端需要连接到 NameNode，NameNode 保存了 HDFS 的所有元数据。然后，如果客户端请求读取，NameNode 会将客户端重定向到实际数据所在的 DataNode(s)；如果是写入请求，则会将客户端重定向到可用资源（即存储空间）的 DataNode(s) 以写入数据。

因此，要从 HDFS 读取数据，客户端需要 NameNode 地址和要读取的文件路径。用户可以按如下方式在 Spark 中从 HDFS 读取文件：

```
JavaRDD<String> hadoopFile = jsc.textFile("hdfs://" + "namenode-host:port/" + "absolute-file-path");
```

例如：

```
JavaRDD<String> hadoopFile = jsc.textFile("hdfs://localhost:8020/user/spark/data.csv");
```

由于 textFile 方法始终返回字符串 RDD，可以以类似方式执行进一步操作：

```
JavaPairRDD<String, Integer> hadoopWordCount = hadoopFile.flatMap(x -> Arrays.asList(x.split(" ")))
    .mapToPair(x -> new Tuple2<String, Integer>((String) x, 1))
    .reduceByKey((x, y) -> x + y);
```

可按如下方式使用 textfile 方法从 HDFS 读取多个文件：

- 要读取多个文件：

```
jsc.textFile("hdfs://" + "namenode-host:port/" + "absolute-file-path-of-file1", "hdfs://" + "namenode-host:port/" + "absolute-file-path-of-file2");
```

- 要读取目录中的所有文件：

```
jsc.textFile("hdfs://" + "namenode-host:port/" + "absolute-file-path-of-dir/*");
```

- 要读取多个目录中的所有文件：

```
jsc.textFile("hdfs://" + "namenode-host:port/" + "absolute-file-path-of-dir1/*" + "," + "hdfs://" + "namenode-host:port/" + "absolute-file-path-of-dir2/*");
```

要将输出写回 HDFS，用户可以按如下方式提供 HDFS URL：

```
hadoopWordCount.saveAsTextFile("hdfs://" + "namenode-host:port/" + "absolute-file-path");
```

## 与 Cassandra 交互

Apache **Cassandra** 是一个开源 NoSQL 数据库。它属于基于 **列族** 的数据库类别。它由 Facebook 开发，并基于 Amazon 的 Dynamo 理念构建。它旨在以分布式方式存储大量数据。

Cassandra 具有高度可扩展性且无单点故障。它以可调一致性满足 CAP 定理中的可用性和分区容忍性类别，也就是说，Cassandra 默认是最终一致的，但是，Cassandra 中的一致性可以以牺牲可用性为代价来提高。Cassandra 旨在支持非常高速的写入，由于其出色的特性，它是 NoSQL 世界中最广泛使用的数据库之一。

要了解更多关于 Cassandra 的信息，可以参阅 https://academy.datastax.com/planet-cassandra。

要从 Spark 与 Cassandra 交互，需要 spark-cassandra-connector。以下是 spark-cassandra-connector 的 Maven 模板：

```
<dependency>
    <groupId>com.datastax.spark</groupId>
    <artifactId>spark-cassandra-connector_2.11</artifactId>
    <version>2.0.0-M1</version>
</dependency>
```

> 前述指定的版本适用于 Spark 2.x.x 版本。用户可以根据需要更改版本。

假设我们在名为 my_keyspace 的键空间中有一个 Cassandra 表，定义如下：

```
CREATE TABLE my_keyspace.emp (
  empid int PRIMARY KEY,
  emp_dept text,
  emp_name text
)
```

)

要读取数据，需要一个 Employee POJO 对象，如下所示：

```
public class Employee implements Serializable{
    private Integer empid;
    private String emp_name;
    private String emp_dept;
    public String toString() {
        return "Employee [empid=" + empid + ", emp_name=" + emp_name + ",
        emp_dept=" + emp_dept + "]";
    }
    public Integer getEmpid() {
        return empid;
    }
    public void setEmpid(Integer empid) {
        this.empid = empid;
    }
    public String getEmp_name() {
        return emp_name;
    }
    public void setEmp_name(String emp_name) {
        this.emp_name = emp_name;
    }
    public String getEmp_dept() {
        return emp_dept;
    }
    public void setEmp_dept(String emp_dept) {
        this.emp_dept = emp_dept;
    }
}
```

Cassandra 表中的列名应与类中的字段名完全匹配，否则会抛出异常。此外，该对象必须是可序列化的。

要将 Spark 连接到 Cassandra，需要将 Cassandra 配置提供给 Spark，如下所示：

```
SparkConf conf = new SparkConf().setMaster("local").setAppName("Cassandra Example");
conf.set("spark.cassandra.connection.host", "127.0.0.1");
JavaSparkContext jsc = new JavaSparkContext(conf);
```

现在，可以使用 spark-cassandra-connector 提供的 CassandraJavaUtil 库将 Cassandra 表的行映射到前面的 POJO 类，如下所示：

```
JavaRDD<Employee> cassandraRDD = CassandraJavaUtil.javaFunctions(jsc).cassandraTable
```

如上述代码片段所示，Cassandra 行已映射到 Employee 对象，这将返回一个员工对象的 RDD。可以在此 RDD 上执行 collect() 操作示例，如下所示：

```
cassandraRDD.collect().forEach(System.out::println);
```

要从表中仅加载一列，可以使用 CassandraJavaUtil 库，如下所示：

```
JavaRDD<String> selectEmpDept = CassandraJavaUtil.javaFunctions(jsc).cassandraTable(
```

此处我们将列 emp_dept 的数据类型用作 mapColumnTo 方法的参数。要了解可以使用 CassandraJavaUtil 库对 Cassandra 数据执行的其他操作，请参阅以下网址：https://github.com/datastax/spark-cassandra-connector。

要将结果写回 Cassandra，可以使用相同的库，如下所示：

```
CassandraJavaUtil.javaFunctions(cassandraTable).writerBuilder("my_keyspace", "emp", CassandraJavaUtil.mapToRow(Employee.class);
```

![](img/a77bd9f099223ea139d6d612f2b90712_239_0.png)

saveToCassandra() 是一个操作。

这种方式有点繁琐。让我们来看看另一种将 Cassandra 表加载为数据集的方式。数据集是带有架构的 RDD。使用数据集 API，可以将 RDD 视为表格结构，因为它同时加载架构和数据。

要将 Cassandra 表加载为数据集，需要创建一个 SparkSession 对象。如第 1 章「Spark 简介」中所述，SparkSession 是 Spark 程序的单一入口点。在 Spark 2.x 之前，需要分别创建 SparkContext、SQLContext 等。然而，在 Spark 2.x 中，SparkSession 承担所有这些角色。

> 我们将在讨论 **Spark SQL** 时，在 *第 8 章*「使用 Spark SQL」中详细介绍数据集和 SparkSession。

让我们从创建 SparkSession 对象开始：

```
SparkSession sparkSession = SparkSession.builder().master("local").appName("My App").config("spark.cassandra.connection.host", "localhost").getOrCreate();
```

下一步是使用 SparkSession 对象从 Cassandra 加载数据：

```
Map<String, String> map = new HashMap<>();
map.put("table", "table_name");
map.put("keyspace", "keyspace_name");
Dataset<Row> cassDataset = sparkSession.read().format("org.apache.spark.sql.cassandra");
```

如上述代码所示，它接受一个映射对象，用户可以在其中指定表属性。

现在，类似于 RDD 的 `collect()` 函数，可以在数据集上执行 `show()` 函数来读取数据：

```
cassDataset.show();
```

> `show()` 函数是一个操作。

这将返回带有列名的数据。因此，由于也提供了架构，解读数据会更加容易。此外，可以在数据集上创建临时视图，用户可利用该视图对数据运行 SQL：

```
cassDataset.createOrReplaceTempView("cass_data_view");
sparkSession.sql("Select * from cass_data_view");
```

与 Cassandra 类似，其他数据库（例如 MongoDB、HBase 等）也提供了 Spark 连接器，可用于将 Spark 连接到这些数据库。

在本节中，我们学习了如何将 Spark 连接到各种数据源并读取数据。在下一节中，我们将学习如何在 Spark 中处理不同类型的结构化数据。

## 使用不同的数据格式

Apache Spark 广泛支持各种文件格式，无论是原生支持还是通过 Java 或其他编程语言编写的库来支持。压缩文件格式以及 Hadoop 的文件格式与 Spark 集成得非常好。Spark 中广泛使用的一些常见文件格式如下：

### 纯文本和特殊格式文本

可以通过在 SparkContext 上调用 textFile() 函数在 Spark 中读取纯文本。然而，对于特殊格式的文本，例如由空格、制表符、波浪号（-）等分隔的文件，用户需要使用 map() 函数遍历文本的每一行，然后按特定字符（例如，波浪号分隔文件中的波浪号 (-））将其拆分。

假设我们有波浪号分隔的文件，其中包含以下格式的人员数据：

name-age-occupation

让我们将此文件加载为 Person 对象的 RDD，如下所示：

#### Person POJO:

```
public class Person implements Serializable {
private String Name;
private Integer Age;
private String occupation;
public String getOccupation() {
return occupation;
}
public void setOccupation(String occupation) {
this.occupation = occupation;
}
public String getName() {
return Name;
}
public void setName(String name) {
Name = name;
}
public Integer getAge() {
return Age;
}
public void setAge(Integer age) {
Age = age;
}
}
```

#### 使用 Java 7 的纯文本和特殊格式文本示例:

```
JavaRDD<String> textFile = jsc.textFile("Path of the file");
JavaRDD<Person> people = textFile.map( new Function<String, Person>() {
public Person call(String line ) throws Exception {
String[] parts = line.split("--");
Person person = new Person();
person.setName(parts[0]);
person.setAge(Integer.parseInt(parts[1].trim()));
person.setOccupation(parts[2]);
return person ;
}
});
people.foreach(new VoidFunction<Person>() {
@Override
public void call(Person p) throws Exception {
System.out.println(p);
}
});
```

#### 使用 Java 7 的纯文本和特殊格式文本示例:

```
JavaRDD<String> textFile = jsc.textFile( "Path of the file" );
JavaRDD<Person> people = textFile.map(line -> {
String[] parts = line.split("--");
Person person = new Person();
person.setName(parts[0]);
person.setAge(Integer.parseInt(parts[1].trim()));
person.setOccupation(parts[2]);
return person ;
});
people.foreach(p -> System.out.println(p));
```

> POJO 对象应为可序列化，因为 RDD 是分布式数据结构，在转换/操作期间的混洗操作中，数据会通过网络传输。

读取文本文件、创建字符串 RDD 然后应用 map() 函数将文本字段映射到 Java 对象的这种安排，只要在 map() 函数中为每一行创建 Java 对象不是昂贵的过程，就能完美工作。不过，如果总体选择 mapPartitions() 而非 map()，通常会有性能提升，因为它为 RDD 的每个分区提供元素迭代器。之后，在其他部分中，凡是有机会使用 map() 的地方，我们都应作为经验法则考虑 mapPartitions() 在此类场景中的适用性。任何预构建阶段，例如创建数据库连接、打开套接字端口或创建 Web 会话，都会带来一些开销，并且可以重用同一个对象。因此，在所有此类场景中，mapPartitions() 都会比 map() 工作得好得多。

#### Java 7:




## Java 8：

```
JavaRDD<Person> peoplePart = textFile.mapPartitions(new FlatMapFunction<Iterator<String>, Iterator<Person>>() {
    @Override
    public Iterator<Person> call(Iterator<String> p) throws Exception {
        ArrayList<Person> personList = new ArrayList<Person>();
        while (p.hasNext()) {
            String[] parts = p.next().split("~");
            Person person = new Person();
            person.setName(parts[0]);
            person.setAge(Integer.parseInt(parts[1].trim()));
            person.setOccupation(parts[2]);
            personList.add(person);
        }
        return personList.iterator();
    }
});
peoplePart.foreach(new VoidFunction<Person>() {
    @Override
    public void call(Person p) throws Exception {
        System.out.println(p);
    }
});
```

```
JavaRDD<Person> peoplePart = textFile.mapPartitions(p -> {
    ArrayList<Person> personList = new ArrayList<Person>();
    while (p.hasNext()) {
        String[] parts = p.next().split("~");
        Person person = new Person();
        person.setName(parts[0]);
        person.setAge(Integer.parseInt(parts[1].trim()));
        person.setOccupation(parts[2]);
        personList.add(person);
    }
    return personList.iterator();
});
peoplePart.foreach(p -> System.out.println(p));
```

同样，以特定格式保存文本文件也可以通过对 RDD 调用 saveAsTextFile() 方法以类似方式完成。它会将数据写入文本文件，对每个 RDD 元素调用 .toString() 方法，每行写入一个元素：

```
people.saveAsTextFile("Path of output file");
```

输出文件的数量等于 RDD 中的分区数量，因此可以在保存文本文件之前，通过对 RDD 调用带适当数量的 repartition() 方法来控制：

```
people.repartition(1).saveAsTextFile("Path of output file");
```

虽然 repartition() 方法在保存时能实现管理输出文件的任务，但由于数据混洗，这可能是一个代价高昂的过程。Coalesce 是另一个可选方案，可以代替 repartition() 使用，它会尽量避免数据混洗，因此可能更适合此类操作：

```
people.coalesce(1).saveAsTextFile("Path of output file");
```

> 请参阅第 7 章 Spark 编程模型 – 高级 中的“高级 Spark 转换”部分，以详细了解 repartitioning 和 coalesce 转换的工作原理。

## 使用 CSV 数据

CSV 文件可以视为纯文本文件，以逗号作为分隔符，通常会按预期工作。假设我们有以下格式的电影数据：

```
movieId, title, genre
```

让我们将此文件加载为 Movie 对象的 RDD，如下所示：

### Movie POJO：

```
public class Movie implements Serializable {
    private Integer movieId;
    private String title;
    private String genre;

    public Movie() {
    };

    public Movie(Integer movieId, String title, String genre) {
        super();
        this.movieId = movieId;
        this.title = title;
        this.genre = genre;
    }

    public Integer getMovieId() {
        return movieId;
    }

    public void setMovieId(Integer movieId) {
        this.movieId = movieId;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getGenre() {
        return genre;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public static Movie parseRating(String str) {
        String[] fields = str.split(",");
        if (fields.length != 3) {
            System.out.println("The elements are ::");
            Stream.of(fields).forEach(System.out::println);
            throw new IllegalArgumentException("Each line must contain 3 fields while the current line has ::" + fields.length);
        }
        Integer movieId = Integer.parseInt(fields[0]);
        String title = fields[1].trim();
        String genre = fields[2].trim();
        return new Movie(movieId, title, genre);
    }
}
```

## Spark 代码：

### 使用 Java 7 处理 CSV 数据的示例：

```
JavaRDD<Movie> moviesRDD = sparkSession.read().textFile("movies.csv")
    .javaRDD().filter(str -> !(null == str))
    .filter(str -> !(str.length() == 0))
    .filter(str -> !str.contains("movieId"))
    .map(new Function<String, Movie>() {
        private static final long serialVersionUID = 1L;

        public Movie call(String str) {
            return Movie.parseRating(str);
        }
    });
moviesRDD.foreach(new VoidFunction<Movie>() {
    @Override
    public void call(Movie m) throws Exception {
        System.out.println(m);
    }
});
```

### 使用 Java 8 处理 CSV 数据的示例：

```
JavaRDD<Movie> moviesRDD = sparkSession.read().textFile("movies.csv")
    .javaRDD().filter(str -> !(null == str))
    .filter(str -> !(str.length() == 0))
    .filter(str -> !str.contains("movieId"))
    .map(str -> Movie.parseRating(str));
moviesRDD.foreach(m -> System.out.println(m));
```

但是，在某些情况下，例如字段内包含逗号，如果不显式处理此类情况就会失败；或者在某些情况下，人们可能希望推断 CSV 的架构，甚至获取头信息。对于所有此类场景，都可以使用由 **Databricks** 提供的 Spark CSV 库来处理 CSV 数据类型：

```
Dataset<Row> csv_read = sparkSession.read().format("com.databricks.spark.csv")
    .option("header", "true")
    .option("inferSchema", "true")
    .load("movies.csv");
csv_read.printSchema();
csv_read.show();
```

可以通过指定具有 StructField 作为元素的 StructType 架构来覆盖默认推断的架构，以表示 CSV 字段：

```
StructType customSchema = new StructType(new StructField[] {
    new StructField("movieId", DataTypes.LongType, true, Metadata.empty()),
    new StructField("title", DataTypes.StringType, true, Metadata.empty()),
    new StructField("genres", DataTypes.StringType, true, Metadata.empty())
});

Dataset<Row> csv_custom_read = sparkSession.read().format("com.databricks.spark.csv")
    .option("header", "true")
    .schema(customSchema)
    .load("movies.csv");
csv_custom_read.printSchema();
csv_custom_read.show();
```

读取 CSV 文件时可以在 options() 方法中使用的一些功能如下：

- **Header**：设置为 true 时，文件的第一行将用于命名列，且不会包含在数据中。所有类型都将被假定为字符串。默认值为 false。
- **Delimiter**：默认情况下，列使用 "," 分隔，但分隔符可以设置为任何字符。
- **Quote**：默认情况下，引号字符为 "，但可以设置为任何字符。引号内的分隔符会被忽略。
- **Escape**：默认情况下，转义字符为 \，但可以设置为任何字符。转义的引号字符会被忽略。
- **Mode**：确定解析模式。默认值为 Permissive。可能的值为：
  - **PERMISSIVE**：尝试解析所有行：缺失的标记将插入 null，额外的标记将被忽略。
  - **DROPMALFORMED**：删除标记数少于或多于预期的行，或标记与架构不匹配的行。
  - **FAILFAST**：如果遇到任何格式错误的行，则以 RuntimeException 中止。
- **Charset**：默认为 UTF-8，但可以设置为其他有效的字符集名称。
- **InferSchema**：自动推断列类型。它需要对数据进行一次额外遍历，默认值为 false。

可以使用 RDD 上的 `write()` 方法将数据保存到磁盘，并伴随其格式和压缩编解码器（如果有），如下所示：

```
csv_custom_read.write()
    .format("com.databricks.spark.csv")
    .option("header", "true")
    .option("codec", "org.apache.hadoop.io.compress.GzipCodec")
    .save("newMovies.csv");
```

写入 CSV 文件时可以在 `options()` 方法中使用的一些常用功能如下：

- **Header**：设置为 true 时，头信息（来自 DataFrame 中的架构）将写入第一行。
- **Delimiter**：默认情况下，列使用 `,` 分隔，但分隔符可以设置为任何字符。
- **Quote**：默认情况下，引号字符为 `"`，但可以设置为任何字符。这将根据 `quoteMode` 写入。
- **Escape**：默认情况下，转义字符为 `\\`，但可以设置为任何字符。转义的引号字符将被写入。
- **Codec**：保存到文件时要使用的压缩编解码器。应为实现 `org.apache.hadoop.io.compress.CompressionCodec` 的类的完全限定名称，或不区分大小写的缩写名称之一（bzip2、gzip、lz4 和 snappy）。如果未指定编解码器，默认不启用压缩。

除了逗号 (,)，该库还可以通过使用分隔符选项指定该字符，来读取任何字符分隔的文件。





## 使用 JSON 数据

JSON 是一种无架构的人类可读文件格式。Web 和移动应用的兴起使 JSON 成为 API 服务和数据存储中非常流行的 Web 数据交换格式。我们可以通过多种方式读取 JSON 文件，例如将文件作为文本读取，然后按记录进行处理，或者使用原生支持 JSON 的 SparkSession/Spark SQL。

首先，让我们了解如何使用外部 JSON 库/解析器按记录处理 JSON，并假设文件中的每条记录都是 JSON 类型。请考虑包含人员数据的 JSON 文件。以下是示例 JSON 记录：

```
{ "year": "2013", "firstName": "DAVID", "county": "KINGS", "sex": "M", "cid": 272 }
```

让我们按以下方式将此数据加载为 PersonDetails 对象的 RDD：

### PersonDetails POJO:

```
public class PersonDetails implements Serializable {
private Integer cid;
private String county;
private String firstName;
private String sex;
private String year;
private Timestamp dateOfBirth;
public Integer getCid() {
return cid;
}
public void setCid(Integer cid ) {
this.cid = cid;
}
public String getCounty() {
return county;
}
public void setCounty(String county ) {
this.county = county;
}
public String getFirstName() {
return firstName;
}
public void setFirstName(String firstName ) {
this.firstName = firstName;
}
public String getSex() {
   return sex;
}
public void setSex(String sex ) {
   this.sex = sex;
}
public String getYear() {
   return year;
}
public void setYear(String year ) {
   this.year = year;
}
public Timestamp getDateOfBirth() {
   return dateOfBirth ;
}
public void setDateOfBirth(Timestamp dateOfBirth ) {
   this.dateOfBirth = dateOfBirth ;
}
}
```

### 使用 Java 7 处理 JSON 数据示例:

```
RDD<String>textFile=sparkSession.sparkContext().textFile("pep_json.json", 2);
JavaRDD<PersonDetails>mapParser=textFile.toJavaRDD().map(new Function<String, PersonDetails>() {
@Override
public PersonDetails call(String v1) throws Exception {
   return new ObjectMapper().readValue(v1, PersonDetails.class );
}
});
mapParser.foreach( new VoidFunction<PersonDetails>() {
   private static final long serialVersionUID = 1L;
   @Override
   public void call(PersonDetails t) throws Exception {
       System.out.println(t);
   }
});
```

### 使用 Java 8 处理 JSON 数据示例:

```
JavaRDD<PersonDetails>mapParser=textFile.toJavaRDD().map( v1 -> new ObjectMapper().
mapParser.foreach(t -> System.out.println(t));
```

### 解析 JSON 文件的代码流程如下:

- 读取包含 JSON 记录的文件，其中每行必须包含一个独立、自包含的有效 JSON 对象
- 获得 String 类型的 RDD 后，可以使用 map 函数将每行自包含的 JSON 对象解析为 JSON 类的 RDD

处理 JSON 对象的这种方法有某些优势，例如可以过滤掉不需要的数据，并且可以在解析阶段对数据类型及其验证进行处理。我们还拥有修改内容以及按需对数据类型和格式进行报错的权限。然而，在 RDD 上使用 map 函数并非最佳方案，因为在这种情况下，JSON 解析器对象会为每条记录或每一行创建一次，因此一种优化是使用 mapPartitions() 代替 map() 函数，这可以访问分区中的所有记录，而不是像 map() 函数那样一次只访问一条记录。

处理 JSON 的另一种方法是使用 Spark SQL，它具有原生处理 JSON 的能力。Spark SQL 为读取和写入数据提供了 JSON 架构的自动推断。它还可以处理 JSON 数据中的嵌套字段，并使用 DataFrame 直接访问这些字段，无需任何转换：

![](img/a77bd9f099223ea139d6d612f2b90712_253_0.png)

> 我们将在第 8 章《使用 Spark SQL》中详细讨论 Spark SQL。

```
Dataset<Row> json_rec=sparkSession.read().json( "pep_json.json" );
json_rec.printSchema();

json_rec.show();
```

考虑到 JSON 数据中未提及架构定义，Spark SQL 将自动推断 JSON 数据集的架构。如果 JSON 字段是数组，则 Spark 会创建数组的 STRUCT 来表示类型字段。DataFrame 提供 printSchema() 方法，可帮助可视化推断的架构。

如果需要更改推断的架构，Spark 提供了创建自定义架构的选项，然后可以将其应用于要读取的 JSON 文件。在前面的数据集中，时间戳默认被视为字符串，但如果我们想更改此默认行为，可以创建自定义架构来覆盖默认值。可以在加载 JSON 文件时应用架构定义，如下所示：

```
StructType schema = new StructType( new StructField[] {
DataTypes.createStructField("cid", DataTypes.IntegerType, true),
DataTypes.createStructField("county", DataTypes.StringType, true),
DataTypes.createStructField("firstName", DataTypes.StringType, true),
DataTypes.createStructField("sex", DataTypes.StringType, true),
DataTypes.createStructField("year", DataTypes.StringType, true),
DataTypes.createStructField("dateOfBirth", DataTypes.TimestampType, true)
});
Dataset <Row> person_mod = sparkSession.read().schema(schema).json(textFile);
person_mod.printSchema();
person_mod.show();
```

在前面的示例中，JSON 字段 cid 是 long 类型，而 dateOfBirth 是字符串类型（如果考虑推断架构的输出）。但是，可以通过创建带有 StructField 数组及其各自数据类型的显式 StructType 架构来修改推断的架构。明确提及 JSON 架构的优势在于，DataFrame 的元素不会被迭代两次，而解析 JSON 对象时始终会产生这种开销。它还可以使用带有特定数据类型的受限架构来约束数据。

RDD 可以按以下方式以 JSON 格式保存：

```
person_mod.write().format("json").mode("overwrite").save("pep_out.json");
```

## 使用 XML 数据

XML 是 Spark 中另一种广泛使用和覆盖的重要文件格式。处理 XML 的一种方法是将每个 XML 作为 RDD 的字符串读取，然后对每个 XML 进行映射，使用 DOM 解析器或 JAXB 等 Java 库进行解析。虽然这种方法并没有错，但 Spark 还支持 Databricks 提供的一个库，该库可以以分布式方式处理无格式的 XML 文件。此功能与 Spark 对 JSON 的支持形成对比，后者期望每个 JSON 对象均为行内 JSON 格式：

```
HashMap<String, String> params = new HashMap<String, String>();
params.put("rowTag", "food");
params.put("failFast", "true");
Dataset<Row> docDF=sparkSession.read()
    .format("com.databricks.spark.xml")
    .options(params)
    .load("breakfast_menu.xml");
docDF.printSchema();
docDF.show();
```

您会注意到，处理 XML 文件的方法与处理 CSV 文件类似；两个库都遵循类似的结构。在读取 XML 文件时，可以在 options() 方法中使用的一些常见功能包括：

- rowTag: 要将其视为行的 XML 文件的行标记。
- samplingRatio: 用于推断架构的采样比率（0.0 ~ 1）。默认值为 1。
- mode: 解析过程中处理损坏记录的模式。默认值为 PERMISSIVE。
- PERMISSIVE: 遇到损坏记录时，将其他字段设置为 null，并将格式错误的字符串放入由 columnNameOfCorruptRecord 配置的新字段中。当用户设置架构时，它会为额外字段设置 null。
- DROPMALFORMED: 忽略所有损坏记录。
- FAILFAST: 遇到损坏记录时抛出异常。
- columnNameOfCorruptRecord: 存储格式错误字符串的新字段的名称。默认值为 _corrupt_record。
- valueTag: 当元素中具有属性但没有子元素时，用于值的标记。默认值为 _VALUE。
- charset: 默认值为 UTF-8，但可以设置为其他有效的字符集名称。

可以通过指定带有 StructField 作为行元素的自定义 StructType 架构来覆盖推断的架构。RDD 可以使用 RDD 上的 write() 方法并按以下方式带选项保存为 XML 格式：

- path: 写入文件的位置。
- rowTag: 要将其视为行的 XML 文件的行标记。
- rootTag: 要将其视为根的 XML 文件的根标记。
- valueTag: 当元素中具有属性但没有子元素时，用于值的标记。默认值为 _VALUE。
- compression: 保存到文件时要使用的压缩编解码器。应为实现 org.apache.hadoop.io.compress.CompressionCodec 的类的完全限定名称，或不区分大小写的缩写名称之一（bzip2、gzip、lz4 和 snappy）。如果未指定编解码器，则默认为无压缩：

```
docDF.write().format("com.databricks.spark.xml")
.option("rootTag", "food")
.option("rowTag", "food")
.save("newMenu.xml");
```

使用 XML 库的 pom 文件条目如下：

```
<dependency>
<groupId>com.databricks</groupId>
<artifactId>spark-xml_${scala.binary.version}</artifactId>
<version>0.3.<version>
</dependency>





## References

- https://github.com/apache/hadoop/blob/branch-2.4/hadoop-mapreduce-project/hadoop-mapreduce-client/hadoop-mapreduce-client-core/src/main/java/org/apache/hadoop/mapred/FileInputFormat.java#L299-373

- https://github.com/databricks/spark-csv

- https://github.com/databricks/spark-xml

## Summary

在本章的第一部分，我们讨论了如何在 Spark 中从各种数据源加载数据。我们已看到连接到 HDFS、S3 等一些常用数据源的代码示例。在后续部分，我们讨论了处理一些广泛使用的结构化格式中的数据，并附带代码示例。

在下一章中，我们将详细讨论 Spark 集群。我们将详细讨论集群设置过程以及 Spark 提供的一些流行集群管理器。此外，我们将探讨如何在集群模式下调试 Spark 应用程序。
