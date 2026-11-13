# Intersection

It produces an intersection of the elements of source RDDs, that is, the target RDD consists of the elements that are identical in source RDDs.

The following example will produce an intersection of two RDDs of integers:

![](bbox=[0.362, 0.291, 0.632, 0.574])

Intersection transformation can be executed as follows:

```
| intRDD.intersection(intRDD2);
```