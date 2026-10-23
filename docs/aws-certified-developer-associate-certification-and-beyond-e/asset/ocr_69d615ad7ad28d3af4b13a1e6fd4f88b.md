In the preceding diagram, you can see that an **AWS Lambda inventory update function** on the left of the workflow is designed to update the product inventory system as products are sold and replenished.

Each product has its message group ID, and the same SNS FIFO topic is used to push those updates to the SQS queues. The sequence of inventory updates is preserved per message group ID. This means, for example, that the ordering of the message for the *delta phone 8+* product is strictly preserved where the m2 update takes place before the m3 update. Because the workflow uses the SQS FIFO queue, this ordering of messages is still preserved when the messages finally reach the *AWS Lambda retail subscriber function* within the context of the message group ID. The update m2 is still received before m3.

The bottom right-hand corner of the workflow uses an SQS standard queue. Even though the message ordering is preserved till it reaches this queue, there is no guarantee that this ordering will be preserved as it reaches its final destination to the *AWS Lambda analytics subscriber function*.

The next section will explore message filtering when using SNS topics.

# SNS Subscriptions and Message Filtering

Subscribers to an SNS topic will receive every message delivered to the topic. However, some consumers are designed only to process a subset of those messages. For example, in the ordering service discussed previously, you may want to filter messages to the inventory service only if the order has been successfully placed and the transaction is complete. An example is illustrated in *Figure 15.15:*

![](bbox=[0.155, 0.528, 0.813, 0.755])