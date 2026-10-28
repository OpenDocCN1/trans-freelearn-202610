The following diagram shows the differences between simple message forwarding, work distribution, and pub/sub:

![](bbox=[0.22, 0.18, 0.78, 0.58])

Clearly, if you require a reliable broker, then it needs to be highly available. Typically, you would cluster multiple brokers together to provide redundancy. Using a broker also allows you to write custom rules to define which subscribers receive what messages. For example, your payment system may only care about orders, but your logging server may want to get all messages from all systems.

You will want to monitor not only your broker servers but also the length of the queues. In other words, the number of messages in each queue should always be steady and close to zero. If the number of messages in a queue is steadily growing then this might indicate a problem, which your operations team will need to resolve. It may be that your consumers can't process messages faster than your producers are sending them and you must construct additional consumers. This could be automated and your monitoring software could spin up an extra instance to scale your system, meeting a temporary spike in demand.