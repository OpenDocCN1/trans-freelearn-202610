Each routing key pattern of topic consumer matches with a routing key in a message that is distributed to a queue of the consumer. The character # matches any number of characters.

We are using the future to run four consumers concurrently:

```
(def f1 (future (topic-consumer "topic.exchange1" "p1.group1.jp" "my.queue1" :num 1)))
;;=> starting consumer ...
;;=> #'chapter04.rabbitmq/f1
(def f2 (future (topic-consumer "topic.exchange1" "p2.group1.jp" "my.queue2" :num 2)))
;;=> starting consumer ...
;;=> #'chapter04.rabbitmq/f2
(def f3 (future (topic-consumer "topic.exchange1" "#.group1.jp" "my.queue3" :num 3)))
;;=> starting consumer ...
;;=> #'chapter04.rabbitmq/f3
(def f4 (future (topic-consumer "topic.exchange1" "#.group2.jp" "my.queue4" :num 4)))
;;=> starting consumer ...
;;=> #'chapter04.rabbitmq/f4
```

# Testing the topic consumers

We will test topic consumers by publishing messages. In the following case, the message of the routing key is p1.group1.jp. So, it matches p1.group1.jp and #.group1.jp:

```
(topic-producer "topic.exchange1" "p1.group1.jp" "hello p1.group1.jp")
;;=> producing message to exchange topic.exchange1 / p1.group1.jp payload = hello p1.group1.jp
;;=> [3-1] hello p1.group1.jp
;;=> [1-1] hello p1.group1.jp
```

Similarly, the routing key p2.group1.jp matches p2.group1.jp and #.group1.jp since # is a wildcard:

```
(topic-producer "topic.exchange1" "p2.group1.jp" "hello p2.group1.jp 1")
```