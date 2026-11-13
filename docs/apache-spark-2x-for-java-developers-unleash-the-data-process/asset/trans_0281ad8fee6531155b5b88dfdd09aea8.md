
follows:

```
unsortedPairRDD.sortByKey(false);
```

此函数还有其他可用的重载，允许用户为键类型提供一个 `Comparator`。因此，如果键未实现 Comparable，可以使用此函数：

```
sortByKey(Comparator<T> comp, boolean ascending)
```

此转换还需要在分区之间进行数据混洗。随着 DataFrames 和 Datasets 的引入，也可以使用 Spark SQL 操作对数据进行排序（这些操作将在第 8 章《使用 Spark SQL》中详细讨论）。

## Join

它用于连接 Pair RDD。此转换与数据库中的 Join 操作非常相似。假设我们有两个类型为 <x,y> 和 <x,z> 的 Pair RDD。当在这些 RDD 上执行 Join 转换时，它将返回类型为 <x,(y,z)> 的 RDD。

以下是 join 操作的示例：

首先创建两个 PairRDD：

```
JavaPairRDD<String, String> pairRDD1 = javaSparkContext.parallelizePairs(Arrays.asL
JavaPairRDD<String, Integer> pairRDD2 = javaSparkContext.parallelizePairs( Arrays.a
```

![](img/a77bd9f099223ea139d6d612f2b90712_195_0.png)

现在，pairRDD1 将与 pairRDD2 连接，如下所示：

```
JavaPairRDD<String, Tuple2<String, Integer>> joinedRDD = pairRDD1.join(pairRDD2);
```

还支持 LeftOuterJoin、RightOuterJoin 和 FullOuterJoin，可以按以下方式执行：

```
pairRDD1.leftOuterJoin(pairRDD2);
pairRDD1.rightOuterJoin(pairRDD2);
pairRDD1.fullOuterJoin(pairRDD2);
```

对于跨多个节点分区的 RDD，相同的键将被混洗到某个执行器节点以执行连接操作。因此，所有连接操作都提供了重载，并允许用户提供分区器：

```
join(JavaPairRDD<K,V> other,Partitioner partitioner)
leftOuterJoin(JavaPairRDD<K,V> other,Partitioner partitioner);
rightOuterJoin(JavaPairRDD<K,V> other,Partitioner partitioner);
fullOuterJoin(JavaPairRDD<K,V> other,Partitioner partitioner);
```

## CoGroup

此操作也用于对两个 PairRDD 进行分组。假设我们有两个类型为 <x,Y> 和 <x,Z> 的 PairRDD。当在这些 RDD 上执行 coGroup 转换时，它将返回类型为 <x, (Iterable<Y>,Iterable<Z>)> 的 RDD。此操作也称为 groupwith。

以下是 coGroup 转换的示例。首先创建两个 Pair RDD：

```
JavaPairRDD<String, String> pairRDD1 = javaSparkContext.parallelizePairs(Arrays.asL
JavaPairRDD<String, Integer> pairRDD2 = javaSparkContext.parallelizePairs(Arrays.as
```

![](img/a77bd9f099223ea139d6d612f2b90712_197_0.png)

对这些 RDD 执行 CoGroup 转换可以按以下方式进行：

```
JavaPairRDD<String, Tuple2<Iterable<String>, Iterable<Integer>>> cogroupedRDD = pai
```

或

```
JavaPairRDD<String, Tuple2<Iterable<String>, Iterable<Integer>>> groupWithRDD = pai
```

由于此操作也需要在分区 RDD 上将相同的键混洗到一个执行器，因此提供了可用于提供分区器的重载函数：

```
cogroup(JavaPairRDD<K,V> other,Partitioner partitioner)
```

本节介绍了各种有用的转换、它们的用法及其在 Java 中的实现。在下一节中，我们将学习 Spark 中可在 RDD 上执行的各种动作。

## 常见 RDD 动作

在上一节中，我们探讨了各种会生成另一个 RDD 的 RDD 转换，然而，触发作业计算并返回非 RDD 值的 RDD 操作称为动作。之前讨论的所有转换在调用动作之前都不会被求值。不同的转换仅有助于物化惰性求值的 DAG。动作是唯一触发将数据加载到 RDD、计算转换，并将最终结果发送到 Driver 程序或写入文件系统的操作。

Spark 中广泛使用的一些常见动作将如下讨论：

### isEmpty

如果 RDD 不包含任何元素，isEmpty 将返回布尔值 True。需要注意的是，即使 RDD 有一个或多个分区，它也可能为空。在以下示例中，我们创建了包含三个整数的 RDD，然后过滤掉父 RDD 中不存在的元素，从而生成一个空 RDD：

### Java 7：

```
// isEmpty
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,2,3));
boolean isRDDEmpty = intRDD.filter(new Function<Integer, Boolean>() {
@Override
public Boolean call(Integer v1) throws Exception {
return v1.equals(5);
}
}).isEmpty();
System.out.println("The RDD is empty ::"+isRDDEmpty);
```

### Java 8：

```
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,2,3));
boolean isRDDEmpty = intRDD.filter(a -> a.equals(5)).isEmpty();
System.out.println("The RDD is empty ::"+isRDDEmpty);
```

### collect

Collect 会将 RDD 不同分区中的数据检索到驱动程序中的数组中。应谨慎仅在数据量相当小的 RDD 上调用 collect 动作，否则可能会导致驱动程序崩溃。

通常，在应用一组转换后，当数据量缩小时，会调用 collect 动作。调用 collect() 的最简单示例如下，即过滤特定值：

```
//Collect
List<Integer> collectedList= intRDD.collect();
for(Integer elements: collectedList){
    System.out.println("The collected elements are ::"+elements);
}
```

### collectAsMap

collectAsMap 动作用于在 PairRDD 上调用，以在驱动程序中以键/值格式收集数据集。由于 RDD 的所有分区数据都会被转储到驱动程序，因此应尽量减小数据大小：

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

在示例中，可以注意到 RDD 中有两个具有相同键的元组，只要不将其作为 Map 收集，它就能正常工作，因为在这种情况下，键的一个值会被另一个值覆盖。

### count

count 方法返回 RDD 跨分区存在的元素数量：

```
//count()
long countVal=intRDD.count();
System.out.println("The number of elements in the RDD are :"+countVal);
```

### countByKey

countByKey 是对 count() 动作的扩展，它用于 Pair RDD 以计算 Pair RDD 中键的出现次数。CountByKey() 方法返回一个 Map，其键为 RDD 元素的键，值为该键的计数：

```
//CountByKey
List<Tuple2<String,Integer>> list= new ArrayList<Tuple2<String,Integer>>();
list.add(new Tuple2<String,Integer>("a", 1));
list.add(new Tuple2<String,Integer>("b", 2));
list.add(new Tuple2<String,Integer>("c", 3));
list.add(new Tuple2<String,Integer>("a", 4));
JavaPairRDD<String, Integer> pairRDD = sparkContext.parallelizePairs(list);
Map<String, Long> countByKeyMap= pairRDD.countByKey();
for( Entry<String, Long> entrySet :countByKeyMap.entrySet() ){
System.out.println("The key of Map is : " + entrySet.getKey() + " and the value is : " + entrySet.getValue());
}
```

### countByValue

countByValue() 方法可同时应用于 RDD 和 Pair RDD，但两种情况下返回的对象签名不同。例如，当对 RDD 应用 countByValue() 时，它会统计该 RDD 中具有特定值的元素出现次数，并返回一个以 RDD 元素为键、出现次数为值的 Map：

```
//countByValue
List<Tuple2<String,Integer>> list= new ArrayList<Tuple2<String,Integer>>();
list.add(new Tuple2<String,Integer>("a", 1));
list.add(new Tuple2<String,Integer>("b", 2));
list.add(new Tuple2<String,Integer>("c", 3));
list.add(new Tuple2<String,Integer>("a", 4));
JavaPairRDD<String, Integer> pairRDD = sparkContext.parallelizePairs(list);
Map<Tuple2<String, Integer>, Long> countByValueMap = pairRDD.countByValue();
for(Entry<Tuple2<String, Integer>, Long> entrySet :countByValueMap.entrySet()) {
System.out.println("The key of Map is a tuple having value : "+entrySet.getKey()._1()+" and "+entrySet.getKey()._2()+" and the value of count is : "+entrySet.getValue());
}
```

当对 Pair RDD 应用 countByValue() 时，它会将 RDD 中每个唯一值的计数作为 (value, count) 对的 Map 返回，其中 value 是 RDD 的元组。在代码片段中可以看到，有两个具有相同键（即 a）但值不同的元组。因此，当对它们应用 countByValue() 动作时，它会将具有 a 的两个键视为不同。最终的合并步骤在主节点上本地执行，相当于运行单个 reduce 任务。
