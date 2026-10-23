
## DynamoDB Optimistic Locking

Optimistic locking is designed to prevent you from accidentally overwriting changes made by others and vice versa. This feature requires each item to have an attribute in the table that can act as a version number. Updates to the item can only be successful if you can reference the correct (or latest) version number as a condition for the update.

For example, in the following diagram, without the use of a version number, note that Client B has been successful in updating the price of the product to $15, even though Client A performed an update just moments earlier to $17. While this may be an acceptable change, it could also be an error performed by Client B.

![](img/34ff9b5b5a734dae3450ab701e223c73_240_0.png)

Figure 6.18 – Database updates without optimistic locking

Notice in the preceding diagram, that Client B was able to update the table and set the price to $15. While in certain cases, this may be acceptable, it could also be an error. With optimistic locking, each item will need to have an attribute that can act as a version number. While retrieving an item from the table, the application would make a note of the version number. This allows the application on your client device to then update the item on the table provided that the version number doesn't change in the interim. If there is a version number mismatch because another client was able to update the item, then your update will fail.

In the following diagram, note that Client B's update fails because of the version number mismatch.

![](img/34ff9b5b5a734dae3450ab701e223c73_241_0.png)

Figure 6.19 – Database updates with optimistic locking

When Client A updated the item, the version number was incremented to 2. This meant that as Client B attempted to perform an update, there was a version mismatch and the update failed.

Ultimately, optimistic locking ensures any client-side item you update or delete is the same as the item in the DynamoDB table. Next, let's look at conditional writes.

## DynamoDB Conditional Writes

DynamoDB conditional writes can help you prevent accidental write operations such as PutItem, UpdateItem, and DeleteItem.

A conditional write will only succeed if the attributes of an item match a specific condition. If the condition is not met, then the write operation will fail.

In the following example, a PutItem write operation will only succeed if the Id primary key does not exist – in other words, if the PutItem write operation will not overwrite an existing item:

```
aws dynamodb put-item \
  --table-name ProductCatalog \
  --item file://item.json \
  --condition-expression "attribute_not_exists(Id)"
```

Conditional writes and optimistic locking enable you to ensure that data isn't accidentally written to the table and must be incorporated as part of your overall application design architecture.

In the next section, you will explore some of the additional features of DynamoDB. Starting with **DynamoDB Time to Live (TTL)**.

## DynamoDB Time to Live (TTL)

Often, you have data stored in your tables that has a limited lifespan. Examples of limited lifespans include the following:

- Removal of a user or sensor data after a period of inactivity in an application
- Removal of session data or old log data
- Archiving items to an Amazon S3 data lake using DynamoDB Streams
- Sensitive data that expires after a certain time to fulfill compliance or regulatory requirements

DynamoDB's TTL feature can be used to define a per-item timestamp that specifies when the data is no longer required. DynamoDB can then delete the item from the table, saving on stored data volumes.

> **Note**
> The TTL feature is provided at no extra cost and does not consume any RCUs or WCUs.

This is a highly cost-effective method of reducing total storage volume compared to attempting to manually purge data that is no longer required. The latter requires searching for data and then performing a delete operation, which would cost RCUs and WCUs.

DynamoDB will typically delete expired items within 48 hours of expiration, and these delete operations are also performed on any LSI/GSI indexes.

Next, let's examine another feature known as DynamoDB Streams.

## DynamoDB Streams

DynamoDB Streams is a feature that can capture time-ordered series of item-level operations such as puts and updates in your table. The information is then stored for up to 24 hours and can be consumed by applications for several use cases. You may wish to initiate a process based on the item modifications that were carried out in your table. For example, let's say you have host product information and want to monitor all updates that happen to items in your table where the product color was changed.

![](img/34ff9b5b5a734dae3450ab701e223c73_243_0.png)

Figure 6.20 – DynamoDB Streams example

In the preceding diagram, note the process flow. As the application writes and updates data in the DynamoDB table, changes are recorded in the DynamoDB Streams (if enabled). These changes are stored for 24 hours and can be used to trigger Lambda functions to perform certain actions or process the change. Data can also be written to CloudWatch about the change.

Whenever your application performs an item-level modification, DynamoDB Streams will write a stream record with a primary key attribute of the item that was modified. You can access information such as the actual modification that was made and the before-and-after images of the modified item.

DynamoDB streams can be enabled when you create a table or at a later stage. The StreamSpecification parameter defines how the stream is configured.

When configuring your DynamoDB streams, you can also specify the type of data that will be written to the stream as follows:

- KEYS_ONLY – Key attributes of the modified item are written to the stream
- NEW_IMAGE – The item after modification is written to the stream
- OLD_IMAGE – The entire item before modification is written to the stream
- NEW_AND_OLD_IMAGE – Both the old and new images of the item are written to the stream

Attempting to enable a stream on a table that already has a stream configured will result in a ResourceInUseException error message.

Another key feature offered by DynamoDB is an in-memory caching service that can help deliver 10 times the performance improvement, taking speeds from milliseconds to microseconds. This feature is offered by a service called the Amazon DynamoDB Accelerator (DAX), which you will explore in the next section.

## Improving Performance with DynamoDB Accelerator (DAX)

Amazon DAX is an in-memory caching service designed to help improve read performance for your application. DAX can help your applications achieve performance improvement at sub-milliseconds (microseconds).

DAX runs as a cluster deployed in your VPC and each cluster can support up to 11 nodes (one primary and up to 10 read replicas). You will therefore need to define a VPC to host the DAX cluster or alternatively accept the default deployment in the default VPC of your chosen region. Because a DAX cluster runs in an Amazon VPC, you will also need to configure security groups accordingly. The DAX cluster will require a security group configured to allow inbound traffic on port 8111 to allow your application running on EC2 instances to communicate with the cluster.

The following diagram shows the placement of DAX in a high-level architectural design pattern:

![](img/34ff9b5b5a734dae3450ab701e223c73_244_0.png)

Figure 6.21 – Configuring a DAX cluster

Your application will need to be configured with a DAX client on the EC2 instances that are deployed. The DAX client directs all application read requests to the DAX cluster. The following API calls can be processed by your DAX cluster: GetItem, BatchGetItem, query, and scan. If DAX has the item, then it returns it without accessing DynamoDB. This is known as a cache hit. If, however, DAX does not have the item, otherwise known as a cache miss, it passes it to DynamoDB. When the response is received, the results are returned to the application and written to the cache. Note that this process only applies if the read request is eventually consistent. For strongly consistent reads, DAX passes the requests to DynamoDB, rather than attempting to retrieve data from the cache.

Note that DAX does not support lazy loading. This feature is a caching strategy that only loads data into the cache when necessary. Instead, DAX supports write-through caching.
