a partitioned RDD, an overloaded function is available to provide partitioner:

```
| cogroup(JavaPairRDD<K,V> other,Partitioner partitioner)
```

This section described various useful transformations, their usage, and their implementation in Java. In the next section, we will learn about various actions available in Spark that can be executed on RDDs.