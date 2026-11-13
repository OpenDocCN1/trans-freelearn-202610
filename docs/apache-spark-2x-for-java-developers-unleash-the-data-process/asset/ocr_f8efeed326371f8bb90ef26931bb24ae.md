# Java 8:

```
pairRDD.aggregateByKey(0, (v1, v2) -> {
    if(v2.startsWith("A"))){
    v1+=1;
    }
    return v1;
}, (v1, v2) -> v1+v2);
```

In the above example the value type of the input RDD is string, but the value type of the output RDD is an integer. Like foldByKey, aggregateByKey also provides the following overloaded methods:

```
aggregateByKey(V2 zeroValue, Partitioner partitioner, Function2<V2,V1,V2> seqFunction,
aggregateByKey(V2 zeroValue, int numPartitions, Function2<V2,V1,V2> seqFunction,Fun
```

![](bbox=[0.142, 0.372, 0.786, 0.425])