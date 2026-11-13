

| Partition number:0, key:France, Partition number:1, key:India, Partition number:1, key:

In this section, we looked in detail at RDD partitioning, and we also learnt about different partitioners, including worked examples of each. In the next section, we will learn about some advanced transformations available in Spark.

## Advanced transformations

As stated earlier in this book, if an RDD operation returns an RDD, then it is called a transformation. In Chapter 4, Understanding the Spark Programming Model, we learnt about commonly used useful transformations. Now we are going to look at some advanced level transformations.

### mapPartitions

The working of this transformation is similar to map transformation. However, instead of acting upon each element of the RDD, it acts upon each partition of the RDD. So, the map function is executed once per RDD partition. Therefore, there will one-to-one mapping between partitions of the source RDD and the target RDD.

As a partition of an RDD is stored as a whole on a node, this transformation does not require shuffling.

In the following example, we will create an RDD of integers and increment all elements of the RDD by 1 using mapPartitions:

```java
JavaRDD<Integer> intRDD = jsc.parallelize(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10), 2);
```

**Java 7:**

```java
intRDD.mapPartitions(new FlatMapFunction<Iterator<Integer>, Integer>() {
    @Override
    public Iterator<Integer> call(Iterator<Integer> iterator) throws Exception {
        List<Integer> intList = new ArrayList<>();
        while (iterator.hasNext()) {
            intList.add(iterator.next() + 1);
        }
        return intList.iterator();
    }
});
```

**Java 8:**

```java
intRDD.mapPartitions(iterator -> {
    List<Integer> intList = new ArrayList<>();
    while (iterator.hasNext()) {
        intList.add(iterator.next() + 1);
    }
    return intList.iterator();
});
```

> mapPartitions provide an iterator to access each element of the RDD belonging to the partition.

As stated in the previous chapter, the `mapPartitions` transformation is really useful if an interaction is required with external systems in the map phase, like reading from a database. Consider an instance where the RDD consists of 100 elements divided into three partitions. If, during the map phase, some data needs to be read from a database based on the value of the element, then running a `map` transformation will create 100 connections to the database. However, with `mapPartitions` only three connections will be created. A connection pool mechanism may be created to handle such scenarios. However `mapPartitions` implicitly provides an advantage in such situations. In the following example, we are reading rows from MySQL table based on every element of the RDD:

```java
intRDD.mapPartitions(iterator -> {
    Class.forName("com.mysql.jdbc.Driver");
    Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/test", "r
    Statement stmt = con.createStatement();
    List<String> names = new ArrayList<>();
    while (iterator.hasNext()) {
        ResultSet rs = stmt.executeQuery("select name from emp where empId = " + iterator.n
        names.add(rs.getString("empName"));
    }
    return names.iterator();
});
```

Since `mapPartitions` is being used, we can iterate over all elements of RDD using the same connection. Had it been a `map` transformation, one connection to MYSQL per element would have been created.

The `mapPartitions` transformation provides an overload where it helps the user to provide a Boolean parameter which decides whether the partitioning information should be preserved or not. The following is the Scala declaration for the same:

```scala
def mapPartitions[U](f: FlatMapFunction[JIterator[T], U], preservesPartitioning: Bo
```

As per the Spark documentation, the value of this parameter should be false unless `mapPartitions` is being performed on a pair RDD and the input function does not modify the keys.

Partitioning information of an RDD is stored in the RDD object itself. An RDD class contains a reference to an instance of the partitioner class. This information is lost after map transformation is performed, which seems logical as the value of the element is being transformed. This holds true in the case of pair RDDs as well as Spark does not preserve partitioning information by default. The mapPartitions creates an instance of the com.javachen.spark.examples.rdd.MapPartitionsRDD class, to which the value of preservesPartitioning is passed as a constructor parameter, the default value of which is false.

The map transformation also instantiates com.javachen.spark.examples.rdd.MapPartitionsRDD with the default value of preservesPartitioning, which is false. On the other hand, a filter transformation instantiates MapPartitionsRDD with preservesPartitioning value set to true. So partitioning information remains preserved in the case of filter transformation.

### mapPartitionsWithIndex

This is similar to mapPartitions. However, in this transformation, the value of the partition index is also available. We showed an example of this transformation in the first section of this chapter (RDD partitioning) while discussing partitioners.

In the following example, a mapPartitionsWithIndex transformation is performed over int RDD to find out which elements belong to which partition:

**Java 7:**

```java
intRDD.mapPartitionsWithIndex(new Function2<Integer, Iterator<Integer>, Iterator<St
    @Override
    public Iterator<String> call(Integer index, Iterator<Integer> iterator) throws Ex
    List<String> list = new ArrayList<String>();
    while (iterator.hasNext()) {
        list.add("Element " + iterator.next() + " belongs to partition " + index);
    }
    return list.iterator();
    }, false);
```

**Java 8:**

```java
intRDD.mapPartitionsWithIndex((index, iterator) -> {
    List<String> list = new ArrayList<String>();
    while (iterator.hasNext()) {
        list.add("Element " + iterator.next() + " belongs to partition " + index);
    }
    return list.iterator();
}, false);
```

It also accepts a Boolean parameter which signifies the value of preservesPartitioning.

### mapPartitionsToPair

This is the cumulative form of mapPartitions and mapToPair. Like mapPartitions, it runs map transformations on every partition of the RDD, and instead of JavaRDD<T>, this transformation returns JavaPairRDD<K,V>.

In the following example, will convert JavaPairRDD of <String, Integer> type using mapPartitionsToPair:

**Java 7:**

```java
JavaPairRDD<String, Integer> pairRDD = intRDD.mapPartitionsToPair(new PairFlatMapFunction
    @Override
    public Iterator<Tuple2<String, Integer>> call(Iterator<Integer> t) throws Exception
    List<Tuple2<String, Integer>> list = new ArrayList<>();
    while (t.hasNext()) {
        int element = t.next();
        list.add(element % 2 == 0 ? new Tuple2<String, Integer>("even", element) : new Tuple2<String, Integer>("odd", element));
    }
    return list.iterator();
});
```

**Java 8:**

```java
JavaPairRDD<String, Integer> pairRDD = intRDD.mapPartitionsToPair(t -> {
    List<Tuple2<String, Integer>> list = new ArrayList<>();
    while (t.hasNext()) {
        int element = t.next();
        list.add(element % 2 == 0 ? new Tuple2<String, Integer>("even", element) : new Tuple2<String, Integer>("odd", element));
    }
    return list.iterator();
});
```

Like mapPartitions and mapPartitionsWithIndex, mapPartitionsToPair also provides an overloaded method that helps user to specify value for preservePartitioning.

### mapValues

mapValues is applicable only for pair RDDs. As its name indicates, this transformation only operates on the values of the pair RDDs instead of operating on the whole tuple.

Acting on a pair RDD, it runs a map operation on the RDD values. This transformation is really useful if the user is only interested in transforming the values on a pair RDD.

The following is an example of mapValues transformation:

**Java 7:**

```java
pairRDD.mapValues(new Function<Integer, Integer>() {
    @Override
    public Integer call(Integer v1) throws Exception {
        return v1 * 3;
    }
});
```

**Java 8:**

```java
pairRDD.mapValues(v1 -> v1 * 3);
```

Another useful fact of mapValues is that it preserves the partitioning information; that is, it instantiates com.javachen.spark.examples.rdd.MapPartitionsRDD with the value of preservesPartitioning set to true, which makes sense since this transformation is only operating on values.

