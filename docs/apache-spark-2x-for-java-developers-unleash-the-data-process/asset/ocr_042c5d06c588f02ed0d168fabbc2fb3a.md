+   - latest: This automatically reset the offset to the latest offset
+   - none: This throws exception to the consumer if no previous offset is found for the consumer's

Before moving any further let's fist have an understanding as to how a basic Kafka consumer will be written in Spark Streaming. As a use case we will try and consume tweets from a Kafka streaming source and count the hashtags within each micro batch interval:

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
tweetRDD.foreachRDD(tRDD -> tRDD.foreach(x->System.out.println(new Date())"+ 
JavaDStream<String> hashtagRDD = tweetRDD.flatMap(twt-> Arrays.stream(twt.sp
hashtagRDD.foreachRDD(tRDD -> tRDD.foreach(x->System.out.println(x)));
JavaPairDStream<String, Long> cntByVal = hashtagRDD.countByValue();
cntByVal.foreachRDD(tRDD -> tRDD.foreach(x->System.out.println(new Date())"+
```

As discussed in the previous section, for any streaming application we need a StreamingContext and the same is true for Kafka as well. After creating StreamingContext one should define the configuration parameter for the Kafka connector and group them together in a Map, similarly group all topics in a collection List. Now we use KafkaUtils helper method createDirectStream() to create a DStream object by passing StreamingContext, LocationStrategy, and ConsumerStrategy, which encompass a collection of topics and consumer configuration map. This is sufficient to consume Kafka messages as DStreams, where each message from Kafka has a key and value pair.