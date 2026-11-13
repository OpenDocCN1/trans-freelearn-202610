# mapToPair

It is similar to map transformation; however, this transformation produces PairRDD, that is, an RDD consisting of key and value pairs. This transformation is specific to Java RDDs. With other RDDs, map transformation can perform both (map and mapToPair()) of the tasks.

Being similar to map transformation, it also creates one-to-one mapping between elements of source RDD and target RDD. So the number of elements of target RDD is equal to the number of elements of source RDD.

The following example of this transformation will convert intRDD created in Map transformation section to JavaPairRDD of <String, Integer> type, that is, the key of target RDD will be of string type and value will be of integer type with key specifying whether the element is even or odd:

![](bbox=[0.29, 0.49, 0.71, 0.74])

Java 7:

```
```java
JavaPairRDD<String, Integer> pairRDD= intRDD.mapToPair(new PairFunction<Integer,
String, Integer>() {
@Override
public Tuple2<String, Integer> call(Integer i) throws Exception {
return (i % 2 == 0) ? new Tuple2<String, Integer>("even", i) : new
Tuple2<String,
Integer>("odd", i);
}
}
```
```