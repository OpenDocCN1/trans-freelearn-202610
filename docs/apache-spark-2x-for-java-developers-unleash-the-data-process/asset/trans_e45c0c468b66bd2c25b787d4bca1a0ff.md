

## 流式转换

与之前的单词计数示例一样，我们看到每行中的单词都只被计数一次，对于下一组记录，计数器会重置。但如果我们想要将之前状态的计数添加到即将到来的下一批次的新单词中呢？我们可以做到吗？如何做到？问题的第一部分答案是，在 Spark Streaming 中有两种转换：有状态转换和无状态转换，因此如果我们想要保留之前的状态，就必须选择有状态转换，而不是我们在之前示例中实现的无状态转换。

基于需求，流处理可以是无状态的或有状态的。某些流处理问题可能需要在一段时间内维护状态，而其他问题则不需要。

假设一家航空公司想要实时处理所有活跃航班温度读数的数据。如果航空公司只想实时打印或存储从航班接收到的读数，则无需维护状态，这被视为无状态流处理。另一种场景是，每隔 10 分钟，航空公司想要找出过去一小时内每个活跃航班的平均温度读数。在这种情况下，可以定义一个一小时的窗口，滑动间隔为 10 分钟。因此，在此场景中需要维护状态，这可以被视为窗口时间框架内的有状态流处理。

假设如果一家航空公司想要找出每个航班整个旅程的平均温度读数，即从起飞到降落，那么就需要维护外部状态，因为不同航班的旅程时间不同，且航班也可能延误。因此，无法定义特定的窗口持续时间。此类流式用例也被视为有状态流处理，其状态在 Spark Streaming 作业执行会话期间一直被维护。

让我们使用 Spark Streaming 开发这些用例。

假设，起飞后航班以以下 JSON 格式发送数据：

```
{"flightId":"tz302", "timestamp":1494423926816, "temperature":21.12,"landed":false}
where flightId = unique id of the flight
timestamp = time at which event is generated
temperature = temperature reading at the time of event generation
landed = boolean flag which defines whether flight has been landed or still on the
```

以下 POJO 将用于将消息映射为 Java 对象以供进一步处理：

```
public class FlightDetails implements Serializable {
private String flightId;
private double temp;
private boolean landed;
private long temperature;
}
```

> 在本示例中，我们将从套接字读取消息。在现实场景中，消息应推送到持久的消息代理/队列，例如 Kafka、Kinesis 等等。

## 无状态转换

最简单的无状态转换是 map 函数，其中每个传递的元素都被视为相互独立。在以下示例中，我们将从流中读取传入消息并将其映射为 FlightDetails 对象，如下所示：

```
JavaReceiverInputDStream<String> inStream= jssc.socketTextStream("localhost", 9999)
JavaDStream<FlightDetails> flightDetailsStream = inStream.map(x -> {
ObjectMapper mapper = new ObjectMapper();
return mapper.readValue(x, FlightDetails.class);
});
```

传入消息可以按如下方式打印或存储到 HDFS：

```
flightDetailsStream.print();
flightDetailsStream.foreachRDD((VoidFunction<JavaRDD<FlightDetails>>) rdd -> rdd.sa'
```

一些常见的无状态转换如下表所示：

| 转换 | 描述 |
|---|---|
| flatMap(func) | 返回展平数据的 DStream；即，每个输入项可以返回多个输出项 |
| filter(func) | 过滤满足作为参数传递的函数的记录 |
| join(otherStream, [numTasks]) | 对于拥有 (K, V) 和 (K, X) 对 RDD 的 DStream，它返回拥有 (k, (V,X)) 对 RDD 的新 DStream |
| union(otherStream) | 返回一个拥有两个不同 DStream 元素并集的 DStream |

| 函数 | 描述 |
|---|---|
| count() | 通过计算源 DStream 中每个 RDD 的元素数量，返回一个单元素 RDD 的新 DStream |
| reduce(func) | 聚合函数在返回单元素 RDD 的新 DStream 的同时减少 DStream 的元素 |
| reduceByKey(func, [numTasks]) | 对于对 DStream RDD，它返回 (K, V) 对的新 DStream，其中每个键的值使用给定的 reduce 函数进行聚合 |

## 有状态转换

有状态转换在一段时间内维护状态，该时间长于微批次间隔，可以长达整个 Spark Streaming 作业会话。这是通过在流式应用程序上创建检查点来实现的，尽管严格来说，窗口化（请参阅窗口化部分）可能不需要检查点，但窗口化上的其他操作可能需要。让我们找出如何对流式应用程序进行检查点。

## 检查点

在第 4 章“理解 Spark 编程模型”中，我们讨论了缓存/持久化 RDD 的各种技术，以避免故障时的所有重新计算。相同的技术也可以应用于 DStream 类型。但是，由于流式应用程序旨在全天运行，应用程序也可能因系统故障或网络故障而失败。为了使 Spark Streaming 应用程序能够从这些故障中恢复，它应该被检查点到外部位置，最好是容错存储，例如 HDFS 等等。

Spark Streaming 允许我们检查点以下类型的信息：

- 元数据检查点：这有助于检查点 Spark Streaming 应用程序的元数据，例如提交应用程序时提供的配置、正在处理的数据批次、定义要对传入 DStream 执行的一组操作等等。
- 数据检查点/生成的 DStream 检查点：这有助于在有状态操作的情况下检查点生成的 DStream，因为生成的 DStream 可以是跨多个批次的各种转换 DStream 的组合，并且这个链在流式应用程序中不断增长。因此，它必须被检查点。

在 Spark Streaming 中，有状态转换的情况下，检查点生成的 DStream 是强制性的。但是，用户也可以在无状态转换的情况下创建检查点，以检查点 Spark Streaming 应用程序的元数据或避免重新计算。甚至 RDD 也可以被检查点。

要在 Spark Streaming 应用程序中启用检查点，可以在 Spark Streaming 上下文上调用以下方法：

```
jssc.checkpoint("/path/of/the/checkpoint_directory");
```

建议将检查点保存在容错文件系统中，最好是 HDFS。

两种转换，即基于窗口的和基于完整会话的，可以在 Spark Streaming 中使用状态实现。基于完整会话的有状态转换在任何 Spark Streaming 作业的生命周期中维护会话，在整个生命周期中维护状态的最流行方法之一是 updateStateByKey。但是，Apache Spark 1.6 引入了 updateStateByKey 的一种高效替代方案，它不仅性能更好，而且更容易编码。

让我们计算航班整个旅程的平均温度。由于不同航班的旅程时间不同，并且考虑到航班延误的情况，我们将使用 mapWithState 有状态流处理来解决此用例。

mapWithState 转换可以在 PairDStream 类型上执行，因此让我们将 flightDetailsStream 转换为 PairStream 类型，键为 flightId，值为 flightDetailsObject：

```
JavaPairDStream<String, FlightDetails> flightDetailsPairStream = flightDetailsStream
```

mapWithState 转换接受 org.apache.spark.streaming.StateSpec 的一个对象，它是一个抽象类型。

其实现需要以下内容的实现：

org.apache.spark.api.java.function.Function3 或 org.apache.spark.api.java.function.l

以下是用于创建 StateSpec 对象的 Function3 接口的实现：

```
Function3<String, Optional<FlightDetails>, State<List<FlightDetails>>, Tuple2<String,
    //fetch existing state else create new list.
    List<FlightDetails> details = state.exists() ? state.get() : \
    boolean isLanded = false;
    /*if optional object is present, add it in the list and check set the b
    if (curFlightDetail.isPresent()) {
    details.add(curFlightDetail.get());
    if (curFlightDetail.get().isLanded()) {
    isLanded = true;
    }
```

