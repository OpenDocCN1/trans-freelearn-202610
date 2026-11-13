

## 重新分区

加载 RDD 后，可以使用名为 repartition 的转换来调整 RDD 的分区数量：

```java
JavaRDD<String> textFile = jsc.textFile("test.txt", 3);
System.out.println("Before repartition:" + textFile.getNumPartitions());
JavaRDD<String> textFileRepartitioned = textFile.repartition(4);
System.out.println("After repartition:" + textFileRepartitioned.getNumPartitions());
```

根据前面的代码示例，重新分区前的分区数量为三，重新分区后将增加到四。
repartition 也适用于压缩文件。因此，使用 repartition 函数读取 gzip 文件后，可以增加其分区数量：

```java
JavaRDD<String> textFile = jsc.textFile("test.gz", 3);
System.out.println("Before repartition:" + textFile.getNumPartitions());
JavaRDD<String> textFileRepartitioned = textFile.repartition(4);
System.out.println("After repartition:" + textFileRepartitioned.getNumPartitions());
```

在这种情况下，重新分区前的分区数量为一，重新分区后为四。重新分区时可能会发生 shuffle，因为新分区可能会分布在不同的节点上。

## Spark 如何计算带有 shuffle 的转换（宽转换）的分区数量

像 reduceByKey、groupByKey、aggregateByKey 等转换会导致数据 shuffle，属于同一个键的数据会被 shuffle 到同一个 executor。因此，这一类中的每个转换都提供一个重载函数，允许用户提供自己选择的分区器。

如果用户指定了分区器，转换后创建的分区数量将基于该分区器的逻辑。但是，如果用户未指定分区器，Apache Spark 默认使用 **哈希分区器**。

根据 HashPartitioner，给定键的结果分区由以下公式决定：

```
{hashCode of the key} % {number of partitions}
```

由于 Spark 无法自行确定分区数量，因此它保持：分区数量 = 最大上游 RDD 的分区数量，其中最大上游 RDD 是上游 DAG 中分区数量最多的 RDD。

这也有助于 Spark 避免内存溢出错误，因为转换前后的分区数量将等于最大上游 RDD 的分区数量；也就是说，该 DAG 中某个阶段的最多分区数量。

## 分区器

如前所述，对于 Pair RDD 上的任何 shuffle 操作，键的目标分区由 shuffle 期间使用的分区器决定。shuffle 期间使用的分区器在决定数据 shuffle 量方面起着重要作用。

分区器的基本要求是相似的键应落在同一个分区中。Apache Spark 提供了一个名为 `partitionBy` 的转换，它可以帮助在任何点根据用户的选择对 RDD 进行分区。此外，如前所述，用户还可以在需要 shuffle 的 Pair RDD 转换（如 `reduceByKey`、`aggregateByKey`、`foldByKey` 等）期间提供自己选择的分区器。

Spark 中提供两种内置类型的分区器。Spark 还提供指定自定义分区器的选项。Spark 中可用的分区器实现如下：

- 1. 哈希分区器
- 2. 范围分区器

## 哈希分区器

**哈希分区器**根据键的 **hashCode** 对键进行分区。它使用以下公式确定结果分区：

```
(hashCode of the key) % (total number of partitions)
```

其中 **hashCode** 是 Java 对象的 **hashCode**

让我们使用 HashPartitioner 对整数 RDD 进行分区：

```java
SparkConf conf = new SparkConf().setMaster("local").setAppName("Partitioning");
JavaSparkContext jsc = new JavaSparkContext(conf);
JavaPairRDD<Integer, String> pairRdd = jsc.parallelizePairs(Arrays.asList(new Tupl
```

我们已初始化 `JavaSparkContext` 并创建了 `pairRdd`，其中键为整数类型，值为字符串类型，并且 numPartitions=3。

如果打印先前创建的 RDD 的分区数量，结果应为 3。让我们使用 `HashPartitioner` 对此 RDD 进行分区：

```java
JavaPairRDD<Integer, String> hashPartitioned = pairRdd.partitionBy(new HashPartition
```

org.apache.spark.HashPartitioner 提供一个参数化构造函数，接受一个参数，即分区数量，如下所示：

```
HashPartitioner(int partitions)
```

由于我们将分区计数的值设置为 2，因此生成的 RDD 将有两个分区，可以按以下方式验证：

```java
System.out.println(hashPartitioned.getNumPartitions());
```

根据 HashPartitioner 公式 - (键的 hashCode) % (分区总数) - 键为 (2, 4, 6, 8) 的元组应位于分区 0，键为 (1, 3, 5, 7) 的元组应位于分区 1。

我们可以按以下方式检查键在分区中的分布：

```java
JavaRDD<String> mapPartitionsWithIndex = hashPartitioned.mapPartitionsWithIndex((in
while(tupleIterator.hasNext()){ list.add("Partition number:"+index+", key:"+tupleIte
return list.iterator();}, true);
System.out.println(mapPartitionsWithIndex.collect());
```

我们将在下一节（高级转换）中讨论 mapPartitionsWithIndex 转换

## 范围分区器

范围分区器基于范围对键进行分区。落在同一范围内的所有键都将落在同一分区中。建议数据类型遵循自然排序，例如整数、long 等。

org.apache.spark.RangePartitioner 提供一个接受五个参数的参数化构造函数，如下所示：

```
RangePartitioner(int partitions, RDD<? extends scala.Product2<K, V>> rdd, bool
```

其中：

- **参数 1：** 分区数量
- **参数 2：** 要分区的 RDD
- **参数 3：** 范围的升序/降序
- **参数 4：** 键的排序方法
- **参数 5：** 键的类标记

让我们按以下方式使用 RangePartitioner 对先前创建的 pairRDD 进行分区：

```java
RangePartitioner rangePartitioner = new RangePartitioner(4, org.apache.spark.api.java.JavaPairRDD<Integer, String> rangePartitioned = pairRdd.partitionBy(rangePartitionione
```

这里，我们对排序和类标记参数使用了 Scala 对象。此外，Java RDD 需要转换为 org.apache.spark.rdd.RDD 才能与 RangePartitioner 一起使用。

由于我们将输出分区数量的值设置为 4，因此生成的 RDD 将包含四个分区，根据范围分区器，键的分布逻辑应如下：

- (1, 2) - 分区 0
- (3, 4) - 分区 1
- (5, 6) - 分区 2
- (7, 8) - 分区 3

以下代码可用于检查键的分布：

```java
JavaRDD<String> mapPartitionsWithIndex = rangePartitioned .mapPartitionsWithIndex((.
while(tupleIterator.hasNext()){ list.add("Partition number:"+index+", key:"+tupleIte
return list.iterator();}, true));
System.out.println(mapPartitionsWithIndex.collect());
```

## 自定义分区器

与哈希分区器和范围分区器一起，Apache Spark 提供在需要时指定自定义分区器的选项。要创建自定义分区器，用户需要扩展 org.apache.spark.Partitioner 类并提供所需方法的实现。

假设我们有一个键为 StringType 的 Pair RDD。我们的要求是根据键的长度将所有元组分区到两个分区中——长度为奇数的键应在一个分区中，另一个分区应包含所有长度为偶数的键。

让我们根据这些要求创建自定义分区器：

```java
import org.apache.spark.Partitioner;
public class CustomPartitioner extends Partitioner{
final int maxPartitions=2;
@Override
public int getPartition(Object key) {
return (((String) key).length())%maxPartitions;
}
@Override
public int numPartitions() {
return maxPartitions;
}}
```

现在，我们将使用此分区器对带有 StringType 键的 RDD 进行分区：

```java
JavaPairRDD<String, String> pairRdd = jsc.parallelizePairs(Arrays.asList(new Tuple2:
JavaPairRDD<String, String> customPartitioned = pairRdd.partitionBy(new CustomParti
```

为了验证分区是否按预期工作，我们将按以下方式打印每个分区所属的键：

```java
JavaRDD<String> mapPartitionsWithIndex = customPartitioned .mapPartitionsWithIndex(
while(tupleIterator.hasNext()){ list.add("Partition number:"+index+", key:"+tupleIte
return list.iterator();}, true);
System.out.println(mapPartitionsWithIndex.collect());
```

它将产生以下输出，显示分区器的行为符合预期：

