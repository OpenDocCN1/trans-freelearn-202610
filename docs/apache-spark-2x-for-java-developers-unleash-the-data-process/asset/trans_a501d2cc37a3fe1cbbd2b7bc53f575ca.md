

```
//Calculate average value of the all temperature values
Double avgSpeed = details.stream().mapToDouble(f -> f.getTemperature())
/*Remove the state if flight has landed else update the state with the upd...
if (isLanded) {
    state.remove();
} else {
    state.update(details);
}
//Return the current average
return new Tuple2<String, Double>(flightId, avgSpeed);
});
```

如前所示的代码示例，Function3 包含四个参数，用于创建 StateSpec 对象：

- KeyType: PairStream 的键类型，在我们的示例中为 String。
- Optional<ValueType>: ValueType 的可选对象，在我们的示例中为 FlightDetails。该参数是可选的，因为一个批次可能包含也可能不包含特定键的数据。
- org.apache.spark.streaming.State<T>: 我们将状态存储为 List<FlightDetails>。
- MappedType: 这是用户希望输出的最终类型。由于我们要计算平均温度，因此将其保留为 Double 类型。

> 请务必阅读代码中添加的注释，以了解 Function3 实现的工作原理。

现在，我们将按如下方式对 flightDetailsPairStream 执行有状态转换：

```
JavaMapWithStateDStream<String, FlightDetails, List<FlightDetails>, Tuple2<String, Double>>
mapWithState.print();
```

> 如前一节所定义，每个流式应用程序的末尾都需要添加以下语句以启动流式上下文：
> `jssc.start();`
> `jssc.awaitTermination();`

每次接收到航班消息时，都会计算并打印截至该时间的平均温度，一旦航班着陆，将计算最终平均值并移除状态。

在实际场景中，发送消息的设备可能因某种原因而失败。因此，为了处理此类情况，可以配置超时，如果在特定时间段内未收到任何消息，则将移除状态。

可以在定义 StateSpec 对象时配置超时，如下所示：

```
flightDetailsPairStream.mapWithState(StateSpec.function(mappingFunc).timeout(Durations.minutes(5)))
```

这表示如果设备在五分钟内未发送消息，则将移除状态。用户可以根据需要设置超时限制。

## 窗口操作

在 Spark Streaming 中，传入的消息流会根据一个间隔（称为批处理间隔）进行分组，系统按批处理间隔接收数据批次。因此，如果我们创建了一个批处理间隔为（例如）两分钟的 streamingContext，则输入 DStream 将以每两分钟一个间隔从接收的数据生成 RDD 并独立处理它们。但是，对于诸如每两分钟计算过去八分钟数据的词频，或每六分钟查找过去半小时读数中股票最大值等用例，需要保留之前批处理间隔的 RDD 状态。为了解决这些需要将任何批处理间隔的 RDD 状态维护一定有限时间段的用例，我们使用基于窗口的有状态转换。

窗口由两个参数定义，即 windowDuration 和 slideInterval。如下图所示，定义了长度为 8 分钟的窗口，每两分钟滑动一次。在这种情况下，传入的消息流最初将收集 8 分钟，然后在第 10 分钟，将丢弃前两分钟接收到的消息，并用第 2 分钟到第 10 分钟接收到的消息形成一个窗口，此过程将每两分钟重复一次。

![](img/a77bd9f099223ea139d6d612f2b90712_392_0.png)

因此，根据图示，Window 1 将包含 Message 1 到 Message 5，**Window 2** 将包含 **Message 2** 到 **Message 6**，以此类推，这与无状态转换有根本区别，在无状态转换中，属于一个批次的消息不会出现在另一个批次中。

并非所有窗口进程都需要维护外部检查点，状态可以保存在内存中，但驱动程序失败时存在数据丢失的风险。可以通过调用以下函数在 JavaDStream RDD 上定义传入流上的滑动窗口：

```
window(Duration windowDuration, Duration slideDuration)
```

在窗口进程中，固定时长的窗口会按滑动间隔值不断滑动，但必须注意的是，窗口时长和滑动时长必须是批处理间隔的倍数，因为接收到的数据是按批处理间隔划分的。

窗口流的窗口时长和滑动间隔如下表所示：

| 窗口流（批处理间隔 2 分钟） | 说明 |
| --- | --- |
| jDStream.window(Durations.minutes(8)) | 有效的窗口流 |
| jDStream.window(Durations.minutes(8),Durations.minutes(2)) | 有效的窗口流 |
| jDStream.window(Durations.minutes(12),Durations.minutes(8)) | 有效的窗口流 |
| jDStream.window(Durations.minutes(2),Durations.minutes(2)) | 有效的窗口流 |
| jDStream.window(Durations.minutes(12),Durations.minutes(12)) | 有效的窗口流 |

| 表达式 | 原因 |
| --- | --- |
| jDStream.window(Durations.minutes(5), Durations.minutes(2)) | 无效的窗口流，因为窗口时长不是批处理间隔的倍数 |
| jDStream.window(Durations.minutes(10), Durations.minutes(1)) | 无效的窗口流，因为滑动时长不是批处理间隔的倍数 |

现在让我们在传入的数据流上定义一个窗口，以查找过去一小时内每个活跃航班（正在发送数据的航班）的平均温度，并且此结果应每 10 分钟计算一次：

```
JavaDStream<FlightDetails> window = flightDetailsStream.window(Durations.minutes(60))
```

我们可以按如下方式对窗口执行转换，以查找每个活跃航班的平均温度：

```
JavaPairDStream<String, Double> transformedWindow = window.mapToPair(f -> new Tuple2<String, Double>())
.mapValues(t -> new Tuple2<Double, Integer>(t, 1))
.reduceByKey((t1, t2) -> new Tuple2<Double, Integer>(t1._1() + t2._1(), t1._2() + t2._2()));
transformedWindow.cache();
transformedWindow.print();
```

每次窗口滑动时都会执行此转换，即在此示例中每 10 分钟执行一次。

在窗口操作中，始终建议对 DStream 进行检查点，以便在发生故障时避免重新计算，因为流式应用程序发生故障时存在数据丢失的可能性。

> 如前一节所定义，每个流式应用程序的末尾都需要添加以下语句以启动流式上下文：
> `jssc.start();`
> `jssc.awaitTermination();`

在本节中，我们了解了 Spark Streaming 中的无状态和有状态转换。在下一节中，我们将通过连接到各种数据源来读取数据流，对 Spark Streaming 进行练习。

## 转换操作

迄今为止，我们对 DStreams 进行了转换，但并非所有 RDD 操作都在 DStream API 中可用。transform 操作克服了这种限制，允许在 DStream 上执行 RDD 操作。假设一种场景，其中 DStream 数据可以与 RDD 连接，然后对它们应用一些分析操作。这将流式数据暴露给 RDD 中可用的一整套非典型操作，因此使其功能极其强大。

以此为例，假设我们有一个单词列表，与其他单词相比，这些单词的权重应优先。因此，如果我们运行词频统计示例，如果 RDD 中的单词存在于 DStream 中，则计数应反映来自 RDD 和 DStream 的总和值：

```
JavaStreamingContext streamingContext = new JavaStreamingContext(sparkConf, Duration
List<Tuple2<String, Integer>> tuples = Arrays.asList(new Tuple2<>("hello", 10), new
JavaPairRDD<String, Integer> initialRDD = streamingContext.sparkContext().parallelize
JavaReceiverInputDStream<String> StreamingLines = streamingContext.socketTextStream
JavaDStream<String> words = StreamingLines.flatMap( str -> Arrays.asList(str.split(
JavaPairDStream<String, Integer> wordCounts = words.mapToPair(str-> new Tuple2<>(st
wordCounts.print();

JavaPairDStream<String, Integer> joinedDstream = wordCounts.transformToPair(newFunc
@Override
public JavaPairRDD<String, Integer> call(JavaPairRDD<String, Integer> rdd) throws
JavaPairRDD<String, Integer> modRDD = rdd.join(initialRDD).mapToPair(
newPairFunction<Tuple2<String, Tuple2<Integer, Integer>>, String, Integer>() {
@Override
public Tuple2<String, Integer> call(
Tuple2<String, Tuple2<Integer, Integer>> joinedTuple) throws Exception {
return new Tuple2<>(joinedTuple._1(), (joinedTuple._2()._1() + joinedTuple
});
return modRDD;
}});
joinedDstream.print();
streamingContext.start();
streamingContext.awaitTermination();
```

