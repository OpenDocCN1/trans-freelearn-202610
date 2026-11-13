# # CoGroup

This operation also groups two PairRDD. Consider, we have two PairRDD of <x,Y> and <x,Z> types . When coGroup transformation is executed on these RDDs, it will return an RDD of <x, (Iterable<Y>,Iterable<Z>)> type. This operation is also called groupwith.

The following is an example of coGroup transformation. Let's start with creating two pair RDDs:

```
| JavaPairRDD<String, String> pairRDD1 = javaSparkContext.parallelizePairs(Arrays.asL
| JavaPairRDD<String, Integer> pairRDD2 = javaSparkContext.parallelizePairs(Arrays.as
```

![](bbox=[0.35, 0.415, 0.64, 0.715])

CoGroup transformation on these RDDs can be executed as follows:

```
| JavaPairRDD<String, Tuple2<Iterable<String>, Iterable<Integer>>> cogroupedRDD = pai
```

or

```
| JavaPairRDD<String, Tuple2<Iterable<String>, Iterable<Integer>>> groupWithRDD = pai
```

As this operation also requires shuffling of the same keys to one executor on