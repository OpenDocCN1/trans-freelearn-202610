the exchange with a routing key.

# Topic-consumer

The `topic-consumer` is as follows:

```
(defn topic-consumer [ex topic qname & {:keys [num] :or {num 1}}]
  (let [handler
        (fn [ch {:keys [delivery-tag]} payload]
          (println (format " [%d-%d] %s" (.getChannelNumber ch) delivery-tag (String. payload "UTF-8"))))]
    (with-open [conn (core/connect)
                ch (channel/open conn num)]
      (println "starting consumer ...")
      (queue/declare ch qname {:durable false :auto-delete false})
      (exchange/declare ch ex "topic" {:durable false :auto-delete false})
      (queue/bind ch qname ex {:routing-key topic})
      (consumers/blocking-subscribe ch qname handler {:auto-ack true}))))
;;=> #'chapter04.rabbitmq/topic-consumer
```

The preceding code is the `topic-consumer`. The `langohr.exchange/declare` declares the exchange to be the topic exchange type. Then, the `langohr.queue/declare` declares a consumer queue, and the `_langohr.queue/bind` binds the exchange and queue.

# Running the topic consumers

We will run four topic consumers, and each consumer accepts different routing key patterns as follows:

| Thread no. | Routing key pattern | Queue name |
|---|---|---|
| 1 | p1.group1.jp | my.queue1 |
| 2 | p2.group1.jp | my.queue2 |
| 3 | #.group1.jp | my.queue3 |
|  | #.group2.jp | my.queue4 |