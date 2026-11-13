```
}};
JavaPairRDD<String, Integer> foldByKey = flatMapToPair.foldByKey(0,new
Function2<Integer, Integer, Integer>() {
@Override
public Integer call(Integer v1, Integer v2) throws Exception {
return v1+v2;
}
}});
```

# Java 8:

```
| JavaRDD<String> stringRDD = jsc.parallelize(Arrays.asList("Hello Spark", "Hello Java
| JavaPairRDD<String, Integer> flatMapToPair = stringRDD.flatMapToPair(s -> Arrays.as
| flatMapToPair.foldByKey(0,(v1, v2) -> v1+v2).collect();
| foldByKey also provides following overloaded methods:
| foldByKey(V zeroValue,Partitioner partitionerFunction2<V, V, V> func);
```

Since aggregateByKey involves data shuffling, this method allows user to
provide instance of partitioner i.e Hash Partitioner, Merge Partitioner or
custom partitioner based on requirement:

```
| foldByKey(V zeroValue,int numPartitions,Function2<V, V, V> func);
```

Here, user can provide the number of partitions. In this case, foldByKey will
create an instance of Hash Partitioner with the number of partitions the user
provided and call the preceding variant.