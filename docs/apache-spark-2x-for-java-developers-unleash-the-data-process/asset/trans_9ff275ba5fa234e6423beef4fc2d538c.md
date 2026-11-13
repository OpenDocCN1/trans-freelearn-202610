

执行接收器，而另一个可用于处理 DStream 数据。因此，始终建议使用 `local[n]` 配置，其中 `n` 大于 1，或者更好的做法是使用 `local[*]`。类似逻辑也可扩展到在集群上运行的 Streaming 作业，分配给 Spark Streaming 应用程序的核心数必须多于接收器的数量。

- 此外，请添加以下 Maven 依赖项以使用 Spark Streaming：

```
<dependency>
<groupId>org.apache.spark</groupId>
<artifactId>spark-streaming_2.11</artifactId>
<version>2.1.1</version>
</dependency>
```

> 要在基于 Linux 的机器上通过套接字流式传输数据，可以使用名为 Netcat 的实用程序/命令。使用以下命令创建服务器端套接字以流式传输数据：
> $nc -lk <Port Number>

## 流式数据源

在 Spark Streaming 中，流式数据源分为两类，即基本源和高级源。所有可直接通过 `streamingContext` 获得的源（如文件系统和套接字流）称为**基本源**，而需要依赖链接的源（如 Kafka、Flume 等）称为**高级源**。流式数据源也可基于可靠性定义；如果在接收并复制消息后向源系统发送确认，则此类接收器称为**可靠接收器**，例如 Kafka API。同样，如果系统不向源系统发送确认，则称其为不可靠。

除前面示例中讨论的套接字流式传输外，一些常见的流式数据源将在下一节中介绍。

### fileStream

可以使用 StreamingContext 的 fileStream() API 从任何目录读取数据文件。fileStream() API 接受四个值，即要监视的用于流式传输文件的目录、键类、值类和 InputFormat 类。对于与 Hadoop 兼容且包含文本数据的文件，可以使用以下语法进行流式传输数据：

```
streamingContext.fileStream(inputDirectory, LongWritable.class, Text.class, TextInpu
```

还有一个更简单的 API，它隐藏了所有键、值和文本输入格式详细信息，在读取简单文本文件时非常有用：

```
streamingContext.textFileStream(inputDirectory);
```

与之前的套接字流式传输示例相比，一个重要的不同点是，由于流式传输文件系统数据不需要接收器，因此不需要额外线程来运行接收器进程。处理文件流数据时需要记住的几个要点是：仅考虑在 Spark 作业已启动并处于运行状态后移动的文件，此外，文件一旦移动到流式传输目录后不应再进行更改。

### Kafka

Kafka 是一个发布-订阅消息系统，提供可靠的 Spark Streaming 数据源。借助最新的 Kafka direct API，它在 Kafka 分区与 DStream 生成的 RDD 分区之间提供一对一映射，并可访问元数据和偏移量。由于就 Spark Streaming 而言 Kafka 是高级流式数据源，因此需要在流式应用程序的构建工具中添加其依赖项。在开始 Kafka 集成之前，应在首选的构建工具中添加以下 Maven 构件：

```
groupId = org.apache.spark
artifactId = spark-streaming-kafka-0-10_2.11
version = 2.1.1
```

添加依赖项后，还需要了解 Kafka 设置的一些基本信息，例如 Kafka 托管所在的服务器（bootstrap.servers）以及描述消息的某些基本配置，例如序列化器、组 ID 等。以下是用于描述 Kafka 连接的几个常用属性：

- bootstrap.servers: 此属性描述 Kafka 服务器的主机和端口，多个之间用逗号分隔。
- key.deserializer: 这是用于反序列化来自 Kafka 的消息键的类名。
- value.deserializer: 这指的是反序列化消息值的类。通常，由于消息是文本，因此 StringDeserializer 类足以满足需求。
- group.id: 这唯一标识消息消费者所属的消费者进程组。
- auto.offset.reset: 当我们开始从 Kafka 主题消费消息，但在 Kafka 中没有初始偏移量，或者当前偏移量在服务器上不再存在时使用；以下选项之一有助于 Kafka 策略决定偏移量：
  - earliest: 这会自动将偏移量重置为最早的偏移量
  - latest: 这会自动将偏移量重置为最新的偏移量
  - none: 如果未找到消费者的先前偏移量，这会向消费者抛出异常

在继续之前，我们先来了解在 Spark Streaming 中如何编写基本的 Kafka 消费者。作为用例，我们将尝试从 Kafka 流式数据源消费推文，并统计每个微批次间隔内的标签数量：

```
JavaSparkContext sc = new JavaSparkContext(conf);
JavaStreamingContext streamingContext = new JavaStreamingContext(sc, Durati
Map<String, Object> kafkaParams = new HashMap<>();
kafkaParams.put("bootstrap.servers", "10.0.75.1:9092");
kafkaParams.put("key.deserializer", StringDeserializer.class);
kafkaParams.put("value.deserializer", StringDeserializer.class);
kafkaParams.put("group.id", "use_a_separate_group_id_for_each_strea");
kafkaParams.put("auto.offset.reset", "latest");
Collection<String> topics = Arrays.asList("mytopic", "anothertopic");
SparkConf sparkConf = new SparkConf().setAppName("WordCountSocketEx").setMaster("lo
final JavaInputDStream<ConsumerRecord<String, String>> stream = KafkaUtils.
ConsumerStrategies.<String, String>Subscribe(topics, kaf
JavaPairDStream<String, String> pairRDD = stream.mapToPair(record-> new Tup
pairRDD.foreachRDD(pRDD-> { pRDD.foreach(tuple-> System.out.println(new Dat
JavaDStream<String> tweetRDD = pairRDD.map(x-> x._2())).map(new TweetText())
tweetRDD.foreachRDD(tRDD -> tRDD.foreach(x->System.out.println(new Date())))
JavaDStream<String> hashtagRDD = tweetRDD.flatMap(twt-> Arrays.stream(twt.sp
hashtagRDD.foreachRDD(tRDD -> tRDD.foreach(x->System.out.println(x)));
JavaPairDStream<String, Long> cntByVal = hashtagRDD.countByValue();
cntByVal.foreachRDD(tRDD -> tRDD.foreach(x->System.out.println(new Date())))
```

如前面部分所讨论，对于任何流式应用程序，都需要 StreamingContext，Kafka 也是如此。创建 StreamingContext 后，应定义 Kafka 连接器的配置参数，并将它们组合在一个 Map 中，同样将所有主题组合在一个集合 List 中。现在，我们使用 KafkaUtils 辅助方法 createDirectStream() 创建 DStream 对象，传入 StreamingContext、LocationStrategy 和 ConsumerStrategy，后者包含主题集合和消费者配置映射。这足以将 Kafka 消费为 DStreams，其中来自 Kafka 的每条消息都包含一个键值对。

不过，我们更感兴趣的是包含推文的消息值部分，因此我们会筛选推文文本，然后按空格拆分单词，最后统计每个包含标签的单词。

本节介绍了流式数据源的基础知识。从 Kafka 消费消息的某些方面需要更多细节和用例理解，因此超出了当前上下文的范围。尽管如此，Kafka 和 Spark 文档详细介绍了如何自定义实现偏移策略，以及如何使用检查点（请参阅容错和可靠性部分）和其他机制从故障中恢复状态。在下一节中，我们将学习流式转换，例如有状态和无状态操作。

