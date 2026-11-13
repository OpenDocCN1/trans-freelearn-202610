+   countByKeyApprox: Applied on pairRDD which returns an approximate result within a timeout period. The return value is of type PartialResult and comes with different variations of the result such as high(), low(), mean(), and so on.

```
// countByKeyApprox(long timeout);
PartialResult<Map<String, BoundedDouble>> countByKeyApprx = pairRDD.countByKey
for( Entry<String, BoundedDouble> entrySet: countByKeyApprx.getFinalValue().
System.out.println("Confidence for "+entrySet.getKey()+""
::"+entrySet.getValue().confidence()));
System.out.println("high for "+entrySet.getKey()+""
::"+entrySet.getValue().high()));
System.out.println("Low for "+entrySet.getKey()+""
::"+entrySet.getValue().low()));
System.out.println("Mean for "+entrySet.getKey()+""
::"+entrySet.getValue().mean()));
System.out.println("Final val for "+entrySet.getKey()+""
::"+entrySet.getValue().toString()));
}
```

If the accuracy of the result is more important than the time taken to generate the result, then another variation of countByKeyApprox Can be used, which also takes into consideration the confidence which is a measure of correctness in the generated result.

```
//countByKeyApprox(long timeout, double confidence)
PartialResult<Map<String, BoundedDouble>> countByKeyApprx = pairRDD.countByKey
for( Entry<String, BoundedDouble> entrySet: countByKeyApprx.getFinalValue()
System.out.println("Confidence for "+entrySet.getKey()+""
::"+entrySet.getValue().confidence()));
System.out.println("high for "+entrySet.getKey()+""
::"+entrySet.getValue().high()));
System.out.println("Low for "+entrySet.getKey()+""
::"+entrySet.getValue().low()));
System.out.println("Mean for "+entrySet.getKey()+""
::"+entrySet.getValue().mean()));
System.out.println("Final val for "+entrySet.getKey()+""
::"+entrySet.getValue().toString()));
}
```

+   countByValueApprox : On pair RDD one can also approximate the count of values by using the countByValueApprox() action. This action returns the result within a stipulated timeout with considerable accuracy, which can be checked using the helper method confidence().

```
// countByKeyApprox(long timeout);
PartialResult<Map<Tuple2<String, Integer>, BoundedDouble>> countByValueApprx
for(Entry<Tuple2<String, Integer>, BoundedDouble> entrySet : countByValueApprx
System.out.println("Confidence for key "+entrySet.getKey()._1() +"
and value "+ entrySet.getKey()._2()+"
::"+entrySet.getValue().confidence()));

```