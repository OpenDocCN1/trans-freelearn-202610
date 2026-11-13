# Interaction with local filesystem

It is very straightforward and easy to read data from a local filesystem in Spark. Let's discuss this with examples, as follows:

Let's put first things first. First, create (or reuse) the Maven project described in the previous chapter and create a Java class (with main method) for our application. We will start by creating a JavaSparkContext:

```
SparkConf conf =new SparkConf().setMaster("local").setAppName("Local File system Ex:
JavaSparkContext jsc=new JavaSparkContext(conf);
```

To read a text file in Spark, the textFile method is provided by SparkContext. The following is the signature of the method:

```
textFile(String path)
```

This method can be used to read the file from local filesystems, as follows:

```
JavaRDD<String> localFile = jsc.textFile("absolutepathOfTheFileOnLocalFileSystem");
```

As shown previously, the file will be read as RDD of strings. Now we can run transformation and/or actions on the data of the file. For example, wordcount on this data can be executed as follows:

```
JavaPairRDD<String, Integer> wordCountLocalFile =localFile.flatMap(x -> Arrays.asLi
.mapToPair(x -> new Tuple2<String, Integer>((String) x, 1))
.reduceByKey((x, y) -> x + y);
wordCountLocalFile.collect();
```

![](bbox=[0.145, 0.75, 0.215, 0.8])

As we already know, DAG of Spark operations starts to execute only when an action is performed. Here, reading the file from the local filesystem is also a part of DAG. Therefore, file reading operations will also be performed only when an action is executed, that is, the collect() operation in the preceding case.

The textfile method can be used to read multiple files as follows: