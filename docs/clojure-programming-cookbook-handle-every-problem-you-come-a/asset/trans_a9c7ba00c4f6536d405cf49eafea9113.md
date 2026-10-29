

## 使用 Kafka

Apache Kafka 是一个由 LinkedIn 开发的快速、可扩展、持久且分布式的消息系统。我们将使用 clj-kafka 使用 Kafka。它是一个 Clojure 包装器。

## 准备工作

我们需要为本教程安装 Kafka。Kafka 的安装步骤如下。

## 下载 Kafka 0.8.2.2

首先，使用 wget 命令下载 Kafka 0.8.2.2：

```
$ wget
http://ftp.kddilabs.jp/infosystems/apache/kafka/0.8.2.2/kafka_2.11-0.8.2.2.tgz
```

然后，从下载的 TAR 文件中提取 Kafka 文件：

```
$ tar xzvf kafka_2.11-0.8.2.2.tgz
```

## 启动 ZooKeeper 和 Kafka 服务器

进入 Kafka 目录并运行 ZooKeeper 和 Kafka 服务器：

```
$ cd kafka_2.11-0.8.2.2
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

前述的 ZooKeeper 启动命令将显示以下输出：

![](img/d7b631352a782ee50160cf8eb995152f_255_0.png)

然后，我们将启动 kafka-server：

```
$ bin/kafka-server-start.sh config/server.properties
```

前述命令将显示以下输出：

![](img/d7b631352a782ee50160cf8eb995152f_255_1.png)

## 创建主题

创建主题名称 test：

```
$ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```

![](img/d7b631352a782ee50160cf8eb995152f_256_0.png)

检查是否存在：

```
$ bin/kafka-topics.sh --zookeeper localhost:2181 --list
```

![](img/d7b631352a782ee50160cf8eb995152f_256_1.png)

## 运行控制台消费者

运行控制台消费者以检查 Clojure 生产者是否工作：

```
$ bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test
```

## 使用 clj-kafka

在本教程中，我们将使用 clj-kafka，它是用于 Kafka 的 Clojure 库：

```
(defproject chapter04 "0.1.0"
:dependencies [[org.clojure/clojure "1.8.0"]
                [clj-kafka "0.3.4"]])
```

## 如何操作...

让我们学习如何将消息发布到 Kafka 主题以及如何从主题消费消息。

## 声明命名空间并加载 Clojure 库

首先，我们将声明本教程使用的命名空间和库：

```
(ns chapter04.kafka
  (:require
    [clj-kafka.producer :as p]
    [clj-kafka.core :as core]
    [clj-kafka.consumer.zk :as zk]
    [clj-kafka.new.producer :as new]
    [clj-kafka.consumer.simple :as simple]
    [clj-kafka.admin :as admin]
    ))
;;=> nil
```

## 生产消息

按如下方式将生产者配置的参数定义为映射：

```
(def producer-config
  {"metadata.broker.list" "localhost:9092"
   "serializer.class" "kafka.serializer.DefaultEncoder"
   "partitioner.class" "kafka.producer.DefaultPartitioner"}
  )
;;=> #'chapter04.kafka/producer-config
```

我们将使用 clj-kafka.producer/send-message 生产四条消息。以下代码将字符串发送到 test 主题：

```
(p/send-message (p/producer producer-config)
  (p/message "test" (.getBytes "Hi hello !")))
;;=> nil
(p/send-message (p/producer producer-config)
  (p/message "test" (.getBytes "Hi hello !")))
;;=> nil
(p/send-message (p/producer producer-config)
  (p/message "test" (.getBytes "Hi hello !")))
;;=> nil
(p/send-message (p/producer producer-config)
  (p/message "test" (.getBytes "Hi hello !")))
;;=> nil
```

## 消费消息

现在，让我们看看如何从主题消费消息。以下代码定义了消费者的配置：

```
(def consumer-config {
"zookeeper.connect" "localhost:2181"
"group.id" "test-consumer-group1"
"auto.offset.reset" "smallest"
"zookeeper.connection.timeout.ms" "5000"
"consumer.timeout.ms" "5000"
"auto.commit.enable" "true"
})
;;=> #'chapter04.kafka/consumer-config
```

然后，消费者从 test 主题读取两条消息：

```
(def x (core/with-resource [c (zk/consumer consumer-config)]
zk/shutdown
(doall (take 1 (zk/messages c "test")))
))
;;=> #'chapter04.kafka/x
```

前述代码返回 clj_kafka.core.KafkaMessage 的惰性序列，其有效负载可以通过 value 访问如下：

```
(-> (first x)
:value
(String. "UTF-8")
)
;;=> "Hi hello !"
```

## 如何工作...

现在，我们将看看 Kafka 生产者和消费者如何工作。

## 生产消息

生产者使用 clj-kafka.producer/send-message 以及生产者对象和消息来生产消息。

send-message 的第一个参数是生产者对象。要创建

| zookeeper.connect | "" | Zookeeper 连接字符串，格式为 hostname:port。 |
| group.id | "" | 唯一标识此消费者所属的消费者进程组的字符串。 |
| auto.offset.reset | largest | 当 ZooKeeper 中没有初始偏移或偏移超出范围时要执行的操作：smallest - 自动将偏移重置为最小偏移 largest - 自动将偏移重置为最大偏移 其他 - 向消费者抛出异常 |
| zookeeper.connection.timeout.ms | 6000 | 客户端在建立与 ZooKeeper 的连接时等待的最大时间。 |
| consumer.timeout.ms | -1 | 如果在指定间隔后没有可供消费的消息，则向消费者抛出超时异常。 |
| auto.commit.enable | true | 如果为 true，则定期向 ZooKeeper 提交消费者已获取的消息的偏移。此已提交的偏移将在进程失败时用作新消费者开始的位置。 |

clj-kafka.consumer.zk/messages 返回 clj_kafka.core.KafkaMessage 的惰性序列。KafkaMessage 定义如下：

```
(defrecord KafkaMessage [topic offset partition key value])
```

clj-kafka.core/with-resource 是一个宏，接受以下三个参数：

- binding: 这是消费者连接的绑定
- close function: 函数退出时调用
- body: 这是函数体

由于 ZooKeeper 会记住每个具有相同 group ID 的消费者所在的偏移，因此使用相同 group ID 消费主题会按如下方式递增其偏移：

```
(core/with-resource [c (zk/consumer consumer-config)]
    zk/shutdown
    (doall (take 1 (zk/messages c "test"))))
;;=> (#clj_kafka.core.KafkaMessage{:topic "test", :offset 1,
:partition 0, :key nil, :value #object[["B" 0x2076f724 "
[B@2076f724"]]})
(core/with-resource [c (zk/consumer consumer-config)]
    zk/shutdown
    (doall (take 1 (zk/messages c "test"))))
;;=> (#clj_kafka.core.KafkaMessage{:topic "test", :offset 2,
:partition 0, :key nil, :value #object[["B" 0x4d0d3a0b "
[B@4d0d3a0b"]]})
(core/with-resource [c (zk/consumer consumer-config)]
    zk/shutdown
    (doall (take 1 (zk/messages c "test"))))
;;=> (#clj_kafka.core.KafkaMessage{:topic "test", :offset 3,
:partition 0, :key nil, :value #object[["B" 0x125f8cd0 "
[B@125f8cd0"]]})
```

## 还有更多...

在这里，我们将学习如何创建一个新主题。

## 创建新主题

以下代码创建一个名为 "test-topic" 的新主题：

```
(with-open [zk (admin/zk-client "127.0.0.1:2181")]
    (let [topic "test-topic"]
      (if-not (admin/topic-exists? zk topic)
        (admin/create-topic zk topic
                            {:partitions 1
                             :replication-factor 1
                             :config {"cleanup.policy"
                                      "compact"}})))
)
;;=> nil
```

## 另请参阅

有关 Apache Kafka 主页，请参阅 [http://kafka.apache.org/](http://kafka.apache.org/) 。

有关 Kafka 快速入门指南，请参阅 [http://kafka.apache.org/documentation.html#quickstart](http://kafka.apache.org/documentation.html#quickstart) 。

## 使用 MQTT

正如人们所说，**物联网 (IoT)** 正在带来第三次工业革命。设备将越来越多地相互通信以实时共享信息，而将这些设备连接起来的低功耗、安全协议的需求比以往任何时候都更大。

什么是 MQTT？它是一种 OASIS 批准的标准，机器对机器超轻量级传输协议，用于发布和订阅活动。

MQTT 客户端始终连接到所谓的 MQTT broker，而不是彼此。

MQTT 协议本身基于 TCP/IP 构建，MQTT 客户端和 MQTT broker 都需要 TCP/IP 堆栈。

MQTT 专门为物联网设计，适用于各种场景，例如：

- 临时社交网络
- 需要对从正在接受治疗的患者收集的实时数据进行即时分析的医院
- 通过在偏远和农村地区如海边或山区使用传感器进行环境监测
- 汽车内部监控，以实时收集和发送有关汽车不同部件的信息
- 分布在赌场各处的赌博机，但必须定期与中央服务器通信
- 收银员通过慢速网络发送交易信息并接收价格更新

正如您所见，MQTT 协议是为低内存和低处理能力设备以及受限的网络环境而设计的。

## 准备工作

