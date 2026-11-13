# Partitioning

Partitioning is a special condition of grouping where the elements of the streams are partitioned based on the logic of the predicate function being passed to the Collectors. The predicate function based on a Boolean logic evaluates each element of the stream to either be true or false. The resultant output of the partitioningBy() method is a Map having two keys, true and false, and a list of values of streams associated with each Boolean value depending on the output of the predicate function:

```
//Partition Collectors
Map<Boolean,List<String>>partitioningExample=streamSupplier.get().collect(Collectors.p
System.out.println("Patitioning of elements on the basis of its length : : "+partiti
```