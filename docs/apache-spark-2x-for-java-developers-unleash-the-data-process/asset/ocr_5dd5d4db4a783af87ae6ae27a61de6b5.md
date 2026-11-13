scenarios Spark uses the **Least Recently Used (LRU)** algorithm to drop partitions. However, if such dropped partitions are ever required then it is recomputed again. To overcome such memory constraint limitation one can choose between `MEMORY_ONLY_SER`, which reduces the memory footprint of the data slightly or otherwise can choose from `MEMORY_AND_DIST` storage level variants:

```
//cache() and persist()
JavaRDD<Integer> rdd = sparkContext.parallelize(Arrays.asList(1, 2,
3, 4, 5),3).cache();
JavaRDD<Integer> evenRDD= rdd.filter(neworg.apache.spark.api.java.function.Function:
@Override
public Boolean call(Integer v1) throws Exception {
return (((v1%2)==0)?true:false;
}});
evenRDD.persist(StorageLevel.MEMORY_AND_DISK());
evenRDD.foreach(newVoidFunction<Integer>() {
@Override
publicvoid call(Integer t) throws Exception {
System.out.println("The value of RDD are :"+t);
}});
//unpersisting the RDD
evenRDD.unpersist();
rdd.unpersist();
```

It is important to note that the `cache()` method is just a syntactic sugar for using the `persist()` method with storage level as `MEMORY_ONLY`. Choosing a storage level for RDD persistence is of paramount importance as far as performance of Spark jobs are concerned, but there are no rules that govern the choice to be made. Some of the factors, however, can act as guiding principles to choose a better storage level for any Spark job:

+   - Prefer `MEMORY_ONLY` storage as long as complete RDD can be persisted in memory.
+   - Try `MEMORY_ONLY_SER` if memory required for RDD persistence is slightly more than available, however, this option increases the operation time as some computation gets consumed in serializing and de-serializing the data.
+   - Spilling the RDD data on disk may sometimes be a far more expensive operation than re-computing the partition itself.
+   - Data replication should be chosen for use cases where extremely high throughput is required such that the time lost in recomputation of lost