
## Interaction with Amazon S3

Simple Storage Service, also known as S3, is an auto scalable storage platform provided by Amazon Web Services (AWS). It is a widely used service for storing data. Once connected to the internet, users can store and retrieve any amount of data at any time from S3. Being one of the cheapest storage services available on the internet, it is used extensively to store huge amounts of data. As it is an AWS service, users do not have to worry about its availability and management.

> If you have not used S3, it is suggested that you refer to the AWS S3 documentation (http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html) for more information.

The following filesystem schemes can be used to access S3 data in Spark:

- S3 (URL: s3://): This scheme is bound to the Hadoop File System. In other words, this scheme is the Hadoop implementation of the HDFS, backed up by S3. So, files will be actually stored in blocks using this scheme and as those are stored in blocks, tools other than Hadoop are unable to read them as regular files. This scheme is deprecated now.

> As this scheme is deprecated, we have not considered it for our code examples.

- S3N (URI: s3n://): S3N, aka S3 Native, is the scheme used to read/write files as regular S3 objects. Using this scheme, files written by Hadoop are accessible by other tools and vice versa. It requires the jets3t library to work.

With this scheme, there is an upper limit of 5GB, that is, files larger than 5GB can't be read using this scheme. Although this scheme is widely used, it is not considered for new enhancements anymore.

- S3A (URI: s3a://): This is the latest filesystem scheme. This scheme has removed the upper limit constraint of S3N and provides various performance improvements. This scheme leverages Amazon's libraries to access S3 objects. S3A is also backward compatible with S3N.

As per Amazon documentation, this scheme is recommended for any newer developments.

To read the data from S3, the following AWS credentials are required:

- AWS Access key ID
- AWS Secret access key

Users can configure AWS credentials using any of the following ways:

- The credentials can be set inline in the Spark program, as follows:
  - **Using S3N**

```
jsc.hadoopConfiguration().set("fs.s3n.awsAccessKeyId", "accessKeyID");
jsc.hadoopConfiguration().set("fs.s3n.awsSecretAccessKey", "secretAccessKey");
```

  - **Using S3A**

```
jsc.hadoopConfiguration().set("fs.s3a.awsAccessKeyId", "accessKeyID");
jsc.hadoopConfiguration().set("fs.s3a.awsSecretAccessKey", "secretAccessKey");
```

It can be read using a property file to avoid security issues. These credentials can be set using environment variables, as follows:

```
export AWS_ACCESS_KEY_ID=accessKeyIDValue
export AWS_SECRET_ACCESS_KEY=securityAccessKeyValue
```

Also, to use the S3A filesystem, the hadoop-aws library is required to be added in the classpath. The following is the Maven dependency template for the same:

```
<dependency>
<groupId>org.apache.hadoop</groupId>
<artifactId>hadoop-aws</artifactId>
<version>2.7.1</version>
</dependency>
```

S3 scheme, to read the data from S3, can be provided in Spark as follows:

- Using S3N:

```
JavaRDD<String> s3File = jsc.textFile("s3n://" + "bucket-name" + "/" + "filename");
```

- Using S3A:

```
JavaRDD<String> s3File = jsc.textFile("s3a://" + "bucket-name" + "/" + "filename");
```

Once RDD is created, further operations (transformation/actions) can be performed on the data to get the required results. For example, the following transformations can be executed on it to generate word count:

```
JavaPairRDD<String, Integer> s3WordCount = s3File.flatMap(x -> Arrays.asList(x.split(" ")))
    .mapToPair(x -> new Tuple2<String, Integer>((String) x, 1))
    .reduceByKey((x, y) -> x + y);
```

The textfile method can be used to read multiple files from HDFS as follows:

- To read more than one file:
  - Using S3N

```
jsc.textFile("s3n://" + "bucket-name" + "/" + "path-of-file1", "s3n://" + "bucket-name" + "/" + "path-of-file2");
```

  - Using S3A

```
jsc.textFile("s3a://" + "bucket-name" + "/" + "path-of-file1", "s3a://" + "bucket-name" + "/" + "path-of-file2");
```

- To read all the files in a directory:
  - Using S3N

```
jsc.textFile("s3n://" + "bucket-name" + "/" + "path-of-dir/*");
```

  - Using S3A

```
jsc.textFile("s3a://" + "bucket-name" + "/" + "path-of-dir/*");
```

- To read all the files in multiple directories
  - Using S3N

```
jsc.textFile("s3n://" + "bucket-name" + "/" + "path-of-dir1/*" + "," + "s3n://" + "bucket-name" + "/" + "path-of-dir2/*");
```

  - Using S3A

```
jsc.textFile("s3a://" + "bucket-name" + "/" + "path-of-dir1/*" + "," + "s3a://" + "bucket-name" + "/" + "path-of-dir2/*");
```

- To save the output back to the S3 schemes can be used as follows:
  - Using S3N

```
s3WordCount.saveAsTextFile("s3n://" + "bucket-name" + "/" + "outputDirPath");
```

  - Using S3A

```
s3WordCount.saveAsTextFile("s3a://" + "bucket-name" + "/" + "outputDirPath");
```

## Interaction with HDFS

Hadoop Distributed File System (HDFS), is widely used for storing huge amounts of data. It is the most popular storage system in the big data world. Its scalability, high availability, and fault tolerance make it an ideal filesystem for storing large chunks of data.

We have already explained some basics of HDFS in Chapter 1, Introduction to Spark. Please refer to it for further clarification of its workings.

For reading or writing data on HDFS, the client has to connect to NameNode, which holds all the metadata of HDFS. Then, NameNode redirects the client to the DataNode(s) where the actual data resides if the client has requested to read, or to the DataNode(s) where resources (that is, storage space) are available to write the data in the case of write requests.

So, to read from HDFS, the client requires the NameNode address and path of the file(s) that needs to be read. Users can read a file in Spark from HDFS as follows:

```
JavaRDD<String> hadoopFile = jsc.textFile("hdfs://" + "namenode-host:port/" + "absolute-file-path");
```

For example:

```
JavaRDD<String> hadoopFile = jsc.textFile("hdfs://localhost:8020/user/spark/data.csv");
```

As the textFile method always returns an RDD of Strings, further operations can be done similarly:

```
JavaPairRDD<String, Integer> hadoopWordCount = hadoopFile.flatMap(x -> Arrays.asList(x.split(" ")))
    .mapToPair(x -> new Tuple2<String, Integer>((String) x, 1))
    .reduceByKey((x, y) -> x + y);
```

The textfile method can be used to read multiple files from HDFS as follows:

- To read more than one file:

```
jsc.textFile("hdfs://" + "namenode-host:port/" + "absolute-file-path-of-file1", "hdfs://" + "namenode-host:port/" + "absolute-file-path-of-file2");
```

- To read all the files in a directory:

```
jsc.textFile("hdfs://" + "namenode-host:port/" + "absolute-file-path-of-dir/*");
```

- To read all the files in multiple directories:

```
jsc.textFile("hdfs://" + "namenode-host:port/" + "absolute-file-path-of-dir1/*" + "," + "hdfs://" + "namenode-host:port/" + "absolute-file-path-of-dir2/*");
```

To write the output back to HDFS, users can provide the HDFS URL as follows:

```
hadoopWordCount.saveAsTextFile("hdfs://" + "namenode-host:port/" + "absolute-file-path");
```

## Interaction with Cassandra

Apache **Cassandra** is an open source NoSQL database. It falls in the **Column family** based database category. It is developed at Facebook and built on the concepts on Amazon's Dynamo. It is designed to store large amounts of data in a distributed fashion.

Cassandra is highly scalable with no single point of failure. It satisfies availability and partition tolerance categories of the CAP theorem with tunable consistency, that is, Cassandra is by default eventually consistent, however, consistency in Cassandra can be increased at the cost of availability. Cassandra is made to support very high-speed writes and due to its excellent features, it is one of the most widely used databases in the NoSQL World.

To read more about Cassandra, you can refer to https://academy.datastax.com/planet-cassandra.

To interact with Cassandra from Spark, spark-cassandra-connector is required. The following is the Maven template for spark-cassandra-connector:

```
<dependency>
    <groupId>com.datastax.spark</groupId>
    <artifactId>spark-cassandra-connector_2.11</artifactId>
    <version>2.0.0-M1</version>
</dependency>
```

> The preceding specified version works with Spark 2.x.x versions. Users can change the version as per their requirement.

Consider, we have a table in Cassandra with the following definition in keyspace named my_keyspace:

```
CREATE TABLE my_keyspace.emp (
  empid int PRIMARY KEY,
  emp_dept text,
  emp_name text
)
```
