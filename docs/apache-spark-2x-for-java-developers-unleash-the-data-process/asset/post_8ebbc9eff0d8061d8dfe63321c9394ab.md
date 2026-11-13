
## Word count on RDD

Let's run a word count problem on stringRDD. Word count is the HelloWorld of the big data world. Word count means that we will count the occurrence of each word in the RDD:

So first we will create pairRDD as follows:

```
scala> val pairRDD = stringRDD.map(s => (s, 1))
pairRDD: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[6] at map at <console>
```

The pairRDD consists of pairs of the word and one (integer) where word represents strings of stringRDD.

Now, we will run the reduceByKey operation on this RDD to count the occurrence of each word as follows:

```
scala> val wordCountRDD = pairRDD.reduceByKey((x, y) => x + y)
wordCountRDD: org.apache.spark.rdd.RDD[(String, Int)] = ShuffledRDD[8] at reduceByKey at <console>
```

Now, let's run collect on it to see the result:

```
scala> val wordCountList = wordCountRDD.collect
wordCountList: Array[(String, Int)] = Array((Python, 1), (JavaScript, 1), (Java, 2), (
scala> wordCountList
res3: Array[(String, Int)] = Array((Python, 1), (JavaScript, 1), (Java, 2), (Scala, 1),
```

As per the output of wordCountList, every string in stringRDD appears once except Java, which appeared twice.

It is shown in the following screenshot:

```
scala> val pairRDD = stringRDD.map(s => (s, 1))
pairRDD: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[9] at map at <console>:26

scala> val wordCountRDD = pairRDD.reduceByKey((x, y) => x + y)
wordCountRDD: org.apache.spark.rdd.RDD[(String, Int)] = ShuffledRDD[10] at reduceByKey at <console>:28

scala> val wordCountList = wordCountRDD.collect
wordCountList: Array[(String, Int)] = Array((Python, 1), (JavaScript, 1), (Java, 2), (Scala, 1), (Ruby, 1))

scala> wordCountList
res4: Array[(String, Int)] = Array((Python, 1), (JavaScript, 1), (Java, 2), (Scala, 1), (Ruby, 1))

scala> 
```

## Finding the sum of all even numbers in an RDD of integers

Let's first create an RDD of integers as follows:

```
scala> val intRDD = sc.parallelize(Array(1, 4, 5, 6, 7, 10, 15))
intRDD: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[0] at parallelize at <console>
```

The next step is to filter all the even elements in this RDD. So, we will execute a filter operation on the RDD, as follows:

```
scala> val evenNumbersRDD = intRDD.filter(i => (i % 2 == 0))
evenNumbersRDD: org.apache.spark.rdd.RDD[Int] = MapPartitionsRDD[3] at filter at <console>
```

The preceding operation will fetch those elements for which 2 is a factor, that is, even elements.

Now, we will sum all the elements of this RDD as follows:

```
scala> val sum = evenNumbersRDD.sum
sum: Double = 20.0
scala> sum
res5: Double = 20.0
```

It is shown in the following screenshot:

![](img/a77bd9f099223ea139d6d612f2b90712_133_0.png)

## Counting the number of words in a file

Let's read the file people.txt placed in $SPARK_HOME/examples/src/main/resources:

```
sparkuser@-~$ cd $SPARK_HOME/examples/src/main/resources
sparkuser@/usr/local/spark/examples/src/main/resources$ ls
full_user.avsc kv1.txt people.json people.txt user.avsc users.avro users.parquet
sparkuser@/usr/local/spark/examples/src/main/resources$ cat people.txt
Michael, 29
Andy, 30
Justin, 19
sparkuser@/usr/local/spark/examples/src/main/resources$
```

The textFile() method can be used to read the file as follows:

```
scala> val file = sc.textFile("/usr/local/spark/examples/src/main/resources/people.txt")
file: org.apache.spark.rdd.RDD[String] = /usr/local/spark/examples/src/main/resources/people.txt MapPartitionsRDD[4] at textFile at <console>:24
```

The next step is to flatten the contents of the file, that is, we will create an RDD by splitting each line with , and flatten all the words in the list, as follows:

```
scala> val flattenFile = file.flatMap(s => s.split(", "))
flattenFile: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[5] at flatMap at <console>
```

The contents of flattenFile RDD looks as follows:

```
scala> flattenFile.collect
res5: Array[String] = Array(Michael, 29, Andy, 30, Justin, 19)
```

Now, we can count all the words in this RDD as follows:

```
scala> val count = flattenFile.count
count: Long = 6
scala> count
res2: Long = 6
```

It is shown in the following screenshot:

```
scala> val file = sc.textFile("/usr/local/spark/examples/src/main/resources/people.txt")
file: org.apache.spark.rdd.RDD[String] = /usr/local/spark/examples/src/main/resources/people.txt MapPartitionsRDD[4] at textFile at <console>:24

scala> val flattenFile = file.flatMap(s => s.split(",", -1))
flattenFile: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[5] at flatMap at <console>:26

scala> flattenFile.collect
res1: Array[String] = Array(Michael, 29, Andy, 30, Justin, 19)

scala> val count = flattenFile.count
count: Long = 6

res2: Long = 6

scala> 
```

Whenever any action such as count gets called, the Spark creates a directed acyclic graph (DAG) to depict the lineage dependency of each RDD. Spark provides a debug method toDebugString() to show such lineage dependencies of the RDD:

```
scala> flattenFile.toDebugString
```

It is shown in the following screenshot:

```
scala> val file = sc.textFile("/usr/local/spark/examples/src/main/resources/people.txt")
file: org.apache.spark.rdd.RDD[String] = /usr/local/spark/examples/src/main/resources/people.txt MapPartitionsRDD[5] at textFile at <console>:24

scala> val flattenFile = file.flatMap(s => s.split(",", -1))
flattenFile: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[6] at FlatMap at <console>:26

scala> flattenFile.toDebugString
res2: String =
(2) MapPartitionsRDD[6] at FlatMap at <console>:26 []
 |  /usr/local/spark/examples/src/main/resources/people.txt MapPartitionsRDD[5] at textFile at <console>:24 []
 |  /usr/local/spark/examples/src/main/resources/people.txt HadoopRDD[4] at textFile at <console>:24 []

scala> 
```

The indentations represent the shuffle while the number in the parentheses indicates the parallelism level at each stage.

In this section, we became familiar with some Spark CLI concepts. In the next section, we will discuss various components of Spark job.

## Spark components

Before moving any further let's first understand the common terminologies associated with Spark:

- **Driver**: This is the main program that oversees the end-to-end execution of a Spark job or program. It negotiates the resources with the resource manager of the cluster for delegate and orchestrate the program into smallest possible data local parallel programming unit.
- **Executors**: In any Spark job, there can be one or more executors, that is, processes that execute smaller tasks delegated by the driver. The executors process the data, preferably local to the node and store the result in memory, disk, or both.
- **Master**: Apache Spark has been implemented in master-slave architecture and hence master refers to the cluster node executing the driver program.
- **Slave**: In a distributed cluster mode, slave refers to the nodes on which executors are being run and hence there can be (and mostly is) more than one slave in the cluster.
- **Job**: This is a collection of operations performed on any set of data. A typical word count job deals with reading a text file from an arbitrary source and splitting and then aggregating the words.
- **DAG**: Any Spark job in a Spark engine is represented by a DAG of operations. The DAG represents the logical execution of Spark operations in a sequential order. Re-computation of RDD in case of a failure is possible lineage can be derived from the DAG.
- **Tasks**: A job can be split into smaller units to be operated upon in silos which are called **Tasks**. Each task is executed upon by an executor on a partition of data.
- **Stages**: Spark jobs can be divided logically into stages, where each stage represents a set of tasks having the same shuffle dependencies, that is, where data shuffling occurs. In shuffle map stage the tasks results are input for the next stage where as in result stage the tasks compute the action that started the evaluation of Spark job such as take(), foreach(), and collect().

Following diagram shows logical representation of how different components of Spark application interacts:

![](img/a77bd9f099223ea139d6d612f2b90712_137_0.png)

## How a Spark job gets executed:

- A Spark Job can comprise of a series of operations that are performed upon a set of data. However big or small a Spark job may be, it requires a sparkContext to execute any such job. In the previous examples of working with REPL, one would notice the use of an environment variable called sc, which is how a SparkContext is accessible in an REPL environment.
- SparkContext creates an operator graph of different transformations of the job, but once an action gets called on such a transformation, the graph gets submitted to the DAGScheduler. Depending on the nature of the RDD or resultant being produced with narrow transformation or the wide transformation (those that require the shuffle operation), the DAGScheduler produces stages.
- The DAGScheduler splits the DAG in such a way that each stage comprises of the same shuffle dependency with common shuffle boundaries. Also, stages can either be a shuffle map stage in which case its tasks' results
