+   • To read all the files in a directory:

```
| jsc.textFile("hdfs://"+"namenode-host:port/"+"absolute-file-path-of-dir/"*");
```

+   • To read all the files in multiple directories:

```
| jsc.textFile("hdfs://"+"namenode-host:port/"+"absolute-file-path-of-dir1/*"+
```

To write the output back to HDFS, users can provide the HDFS URL as follows:

```
| hadoopWordCount.saveAsTextFile("hdfs://"+"namenode-host:port/"+"absolute-file-path-
```