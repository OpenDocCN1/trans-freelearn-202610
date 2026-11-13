# saveAsTextFile

The data of RDD can be saved to some local filesystem, HDFS, or any other Hadoop-supported filesystem using the action saveAsTextFile(). Each element of the RDD is first converted to a line of string by calling the toString() function on them before saving it on a filesystem:

Example for saveAsTextFile() using Java 7:

```
//saveAsTextFile()
JavaRDD<Integer> rdd = sc.parallelize(Arrays.asList(1, 2, 3,4,5),3);
intRDD.saveAsTextFile("TextFileDir");
JavaRDD<String> textRDD= sparkContext.textFile("TextFileDir");
textRDD.foreach(new VoidFunction<String>() {
@Override
public void call(String x) throws Exception {
System.out.println("The elements read from TextFileDir are :"+x);
}});
```

Java 8:

```
JavaRDD<Integer> rdd = sc.parallelize(Arrays.asList(1, 2, 3, 4, 5),3);
intRDD.saveAsTextFile("TextFileDir");
JavaRDD<String> textRDD= sparkContext.textFile("TextFileDir");
textRDD.foreach(x->System.out.println("The elements read from TextFileDir are :"+x)
```

The text files on the filesystem can be read using the textFile() method of SparkContext as shown in the example.