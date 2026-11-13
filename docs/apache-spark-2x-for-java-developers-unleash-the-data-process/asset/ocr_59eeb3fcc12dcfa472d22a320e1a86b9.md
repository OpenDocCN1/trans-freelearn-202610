```
<artifactId>hadoop-aws</artifactId>
<version>2.7.1</version>
</dependency>
```

S3 scheme, to read the data from S3, can be provided in Spark as follows:

+   Using S3N:

```
JavaRDD<String> s3File = jsc.textFile("s3n://"+"bucket-name"+"/"+"f.
```

+   Using S3A:

```
JavaRDD<String> s3File = jsc.textFile("s3a://"+"bucket-name"+"/"+"f.
```

Once RDD is created, further operations (transformation/actions) can be performed on the data to get the required results. For example, the following transformations can be executed on it to generate word count:

```
JavaPairRDD<String, Integer> s3WordCount=s3File .flatMap(x -> Array
.mapToPair(x -> new Tuple2<String, Integer>((String)
.reduceByKey((x, y) -> x + y);
```

The textfile method can be used to read multiple files from HDFS as follows:

+   To read more than one file:

+   Using S3N

```
jsc.textFile("s3n://"+"bucket-name"+"/"+"path-of-file1""", "+"s3n://
```

+   Using S3A

```
jsc.textFile("s3a://"+"bucket-name"+"/"+"path-of-file1""", "+"s3a://
```

+   To read all the files in a directory:

+   Using S3N

```
jsc.textFile("s3n://"+"bucket-name"+"/"+"path-of-dir/*");
```

+   Using S3A