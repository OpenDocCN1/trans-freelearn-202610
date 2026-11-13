# # collectAsMap

The collectAsMap action is called on PairRDD to collect the dataset in key/value format at the driver program. Since all the partition data of RDD is dumped at the driver all the effort should be made to minimize the size:

```
```java
// collectAsMap
List<Tuple2<String, Integer>> list= new ArrayList<Tuple2<String, Integer>>();
list.add(new Tuple2<String, Integer>("a", 1));
list.add(new Tuple2<String, Integer>("b", 2));
list.add(new Tuple2<String, Integer>("c", 3));
list.add(new Tuple2<String, Integer>("a", 4));
JavaPairRDD<String, Integer> pairRDD = sparkContext.parallelizePairs(list);
Map<String, Integer> collectMap=pairRDD.collectAsMap();
for(Entry<String, Integer> entrySet :collectMap.entrySet()) {
System.out.println("The key of Map is : "+entrySet.getKey()+" and the value is : "+entrySet.getValue());
}
```
```

In the example, one can notice that the RDD has two tuples having the same key and it works fine as long as it is not collected as Map, as in such a scenario one of the values of key gets overriden by another.