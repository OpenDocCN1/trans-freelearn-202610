
So in this case, the number of elements of target RDD is more than or equal to the number of elements of source RDD.

## mapToPair

It is similar to map transformation; however, this transformation produces PairRDD, that is, an RDD consisting of key and value pairs. This transformation is specific to Java RDDs. With other RDDs, map transformation can perform both (map and mapToPair()) of the tasks.

Being similar to map transformation, it also creates one-to-one mapping between elements of source RDD and target RDD. So the number of elements of target RDD is equal to the number of elements of source RDD.

The following example of this transformation will convert intRDD created in Map transformation section to JavaPairRDD of <String, Integer> type, that is, the key of target RDD will be of string type and value will be of integer type with key specifying whether the element is even or odd:

![](img/a77bd9f099223ea139d6d612f2b90712_182_0.png)

### Java 7

```java
JavaPairRDD<String, Integer> pairRDD = intRDD.mapToPair(new PairFunction<Integer, String, Integer>() {
    @Override
    public Tuple2<String, Integer> call(Integer i) throws Exception {
        return (i % 2 == 0) ? new Tuple2<String, Integer>("even", i) : new Tuple2<String, Integer>("odd", i);
    }
});
```

### Java 8

```java
JavaPairRDD<String, Integer> mapToPair = intRDD.mapToPair(i -> (i % 2 == 0) ? new Tuple2<String, Integer>("even", i) : new Tuple2<String, Integer>("odd", i));
```

## flatMapToPair

It is similar to flatMap(), however, the target RDD is a type of PairRDD, that is, an RDD consisting of key and value pairs. Like mapToPair(), this operation is also specific to JavaRDDs.

Let's extend the example of FlatMap() transformation to create an RDD that consists of a word as the key and length of the word as its value:

![](img/a77bd9f099223ea139d6d612f2b90712_184_0.png)

### Java 7

```java
stringRDD.flatMapToPair(new PairFlatMapFunction<String, String, Integer>() {
    @Override
    public Iterator<Tuple2<String, Integer>> call(String s) throws Exception {
        List<Tuple2<String, Integer>> list = new ArrayList<>();
        for (String token : s.split(" ")) {
            list.add(new Tuple2<String, Integer>(token, token.length()));
        }
        return list.iterator();
    }
});
```

### Java 8

```java
stringRDD.flatMapToPair(s -> Arrays.asList(s.split(" ")).stream().map(token -> new
```

## union

As the name suggests, this transformation unites two RDDs into one. The target consists of all the elements of source RDDs. The following example of union transformation combines two Integer RDDs:

![](img/a77bd9f099223ea139d6d612f2b90712_185_0.png)

We will create an integer RDD using JavaSparkContext and then create union of this RDD with the integer RDD created in the map() transformation section:

```java
JavaRDD<Integer> intRDD2 = javaSparkContext.parallelize(Arrays.asList(1, 2, 3));
intRDD.union(intRDD2);
```

## Intersection

It produces an intersection of the elements of source RDDs, that is, the target RDD consists of the elements that are identical in source RDDs.

The following example will produce an intersection of two RDDs of integers:

![](img/a77bd9f099223ea139d6d612f2b90712_186_0.png)

Intersection transformation can be executed as follows:

```java
intRDD.intersection(intRDD2);
```

## Distinct

As you would have already guessed, the distinct operation will return an RDD by fetching all the distinct elements of a source RDD. Distinct uses the hashCode and equals method of the objects to find distincts:

![](img/a77bd9f099223ea139d6d612f2b90712_187_0.png)

Let's create an RDD of integers that contains duplicates:

```java
JavaRDD<Integer> rddwithdupElements = javaSparkContext.parallelize(Arrays.asList(1;
```

The distinct operation on the RDD can be executed as follows:

```java
rddwithdupElements.distinct();
```

## Cartesian

Cartesian transformation generates a cartesian product of two RDDs. Each element of our first RDD is paired with each element of the second RDD. Therefore, if the cartesian operation is executed on an RDD of types X and an RDD of type Y it will return an RDD that will consist of <x, y> pairs. The resultant RDD will consist of all the possible pairs of <x, y>.

![](img/a77bd9f099223ea139d6d612f2b90712_188_0.png)

Cartesian transformation of RDD of Strings and RDD of integers can be executed as follows:

```java
JavaRDD<String> rddStrings = javaSparkContext.parallelize(Arrays.asList("A", "B", "C"));
JavaRDD<Integer> rddIntegers = javaSparkContext.parallelize(Arrays.asList(1, 4, 5));
rddStrings.cartesian(rddIntegers);
```

The next set of transformation works on PairRDDs:

## groupByKey

groupByKey transformation works with PairRDD (that is, RDD consists of (key, value) pairs). It is used to group all the values that are related to keys. It helps to transform a PairRDD consists of <key, value> pairs to PairRDD of <key, Iterable<value>> pairs.

In the following example, we will execute the groupByKey operation on pairRDD generated in the mapToPair() transformation section.

The groupByKey operation can be executed as follows:

```java
pairRDD.groupByKey()
```

![](img/a77bd9f099223ea139d6d612f2b90712_189_0.png)

There is an overload available for this transformation function that lets the user provide Partitioner:

```java
groupByKey(Partitioner partitioner)
```

As RDD is partitioned across multiple nodes. A key can be present in multiple partitions. In an operation such as groupByKey data will shuffled across multiple partitions. So key after shuffling will land on which of the executor running is decided by Partitioner. By default, Spark uses Hash Partitioner that uses hashCode of key to decide which executor it should be shuffled to. User can also provide custom partitioners based on the requirement.

> RDD Partitioning concepts will be explained in detail in Chapter 7, Spark Programming Model-Advance.

groupByKey() is a very costly operation as it requires a lot of shuffling. Data corresponding to one key is shuffled across the network, which can cause serious issues in case of large datasets.

## reduceByKey

It is another transformation for Pair RDDs that helps to aggregate data corresponding to a key with the help of an associative reduce function. A function is passed as an argument to this transformation, which helps to aggregate the values corresponding to a key.

Given the same PairRDD that we used in groupByKey, for example, the reduceByKey operation can be performed on it as follows:

![](img/a77bd9f099223ea139d6d612f2b90712_191_0.png)

### Java 7

```java
pairRDD.reduceByKey(new Function2<Integer, Integer, Integer>() {
    @Override
    public Integer call(Integer v1, Integer v2) throws Exception {
        return v1 + v2;
    }
});
```

### Java 8

```java
pairRDD.reduceByKey((v1, v2) -> v1 + v2);
```

In the previous example, the reduceByKey transformation will sum all the values corresponding to a key. The function accepts two parameters. The type of both parameters and return type of the function is the same as the value type of PairRDD. The first parameter is the cumulative value and the second value is the value of the pair that is next to be merged.

Similar to group by key, the reduceByKey function also provides an overload that accepts a partitioner as an argument:

```java
reduceByKey(Partitioner partitioner, new Function2<T, T, T>() func)
```

For an RDD with multiple partitions, the reduceByKey transformation aggregates the values at partition level before shuffling, which helps in reducing the data travel of network. It is similar to the concept of combiner in Hadoop MapReduce. For a large dataset it is huge saving in the data being transferred over network. Therefore, reduceByKey is recommended over groupByKey.

## sortByKey

sortByKey() belongs to OrderedRDDFunctions. It is used to sort a pair RDD by its key in ascending or descending order.

To execute sortByKey() on a PairRDD, Key should be of ordered type. In the Java world, key type should have implemented comparable, or else it throws an exception. So for a custom key type, the user should implement comparable interface.

The following is an example of the sortByKey() transformation:

![](img/a77bd9f099223ea139d6d612f2b90712_193_0.png)

Let's first create a pair RDD of <String, Integer> type using JavaSparkContext:

```java
JavaPairRDD<String, Integer> unsortedPairRDD = javaSparkContext.parallelizePairs(
    Arrays.asList(new Tuple2<String, Integer>("B", 2), new Tuple2<String, Integer>("C",
```

Now, let's execute the sortByKey transformation on it to sort it in descending order:

```java
unsortedPairRDD.sortByKey()
```

There is an overload available for the sortByKey() function that takes a Boolean argument:

```java
sortByKey(boolean ascending)
```

This function can be used to sort an RDD in descending order by keys as
