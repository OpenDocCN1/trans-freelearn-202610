As ZooKeeper remembers offsets where each consumer has the same group ID, consuming a topic with the same group ID increments its offset as follows:

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

# There's more...

Here, we will learn how to create a new topic.

# Creating a new topic

The following code creates a new topic named "test-topic":

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

# See also