
- **Using a transaction for a single document update:** Wrapping a single document update in a transaction is unnecessary as single-document operations are already atomic in MongoDB. This redundant approach adds processing overhead without providing any additional data integrity benefits.
- **Using transactions instead of better schema design:** Another common mistake is using transactions when the data could be modeled to fit within a single document. Taking time to design your schema properly can eliminate many transaction requirements and significantly improve performance.
- **Splitting logically related data across collections:** Developers also frequently create separate documents for data that logically belongs together, then use transactions to update them atomically. This approach not only complicates your code but also introduces potential performance bottlenecks and increases the likelihood of transaction conflicts.
- **Ignoring single-document atomicity:** MongoDB’s single-document atomicity is a powerful feature; don’t ignore it! Using multi-document transactions for operations that could be handled with single-document updates adds unnecessary overhead and complexity.

### Tip

Where appropriate, embed related data in a single document to reduce the need for multi-document transactions.

Here’s an example demonstrating the difference between a transaction-heavy relational approach and a document-optimized MongoDB model:

```
// Instead of separate collections requiring transactions:
// users collection: { _id: userId, name: "Jane", ... }
// profiles collection: { userId: userId, preferences: {...}, ... }
// settings collection: { userId: userId, notifications: {...}, ... }

// Consider a consolidated model:
// users collection
{
  _id: userId,
  name: "Jane",
  profile: {
preferences: {"..."}
},
settings: {
  notifications: {"..."}
}
}
```

In the first approach, user information is split across three separate collections: users, profiles, and settings. Each time you need to update related data across these collections, you’d need to use multi-document transactions to ensure consistency. This approach creates unnecessary complexity and performance overhead.

The second approach demonstrates a consolidated model where all user-related information is embedded within a single document in the users collection. With this design, you can update a user’s profile preferences and notification settings in a single atomic operation without needing transactions. This approach simplifies your code, improves performance, and leverages MongoDB’s document model as it was designed to be used.

## Single-document transactions

Making multiple distinct updates to a single document in a transaction can almost always be replaced with a single atomic update. Unless some third-party interaction is involved between the two writes, which is problematic by itself due to the potentially huge increase in latency and risk of exceeding the transaction timeout limit, the correct technique would be to compose a single update (using expressive aggregation syntax if necessary) to make all the modifications in a single operation.

If it’s critical that no other thread is reading a document while one thread is making updates to it, it’s possible to achieve that without a transaction by placing a logical lock field in the document with one update and later when updating the other fields unsetting that field. Take care that the rest of the operations check that no lock exists when they interact with the document.

## Transactions for read-only operations

Using multi-document transactions solely for read operations is an unnecessary overhead that can negatively impact performance. To ensure a consistent snapshot for a series of reads across many documents, use read concern snapshot rather than starting a transaction. If you need to ensure reads reflect your previous writes, use causally consistent sessions or use the linearizable read concern.

## Misunderstanding transaction scope and atomicity

A common anti-pattern is either including too little or too much within a transaction, or misunderstanding what operations are truly atomic. Another common misconception is that a single write command is atomic even when it affects multiple documents. An update operation with the `multi:true` option or the `updateMany` method is not atomic as a whole; only each individual write within it is atomic. To make such a multi-update atomic, it would need to be wrapped in a transaction.

Only include operations that must be atomic together in a transaction. Non-critical operations should be performed outside the transaction to reduce scope and improve performance.

## Frequent small transactions on hot documents/collections

Small transactions are usually a good thing, but running them too often on the same documents can lead to performance issues. This often happens when different services independently update related documents, such as multiple microservices modifying the order status and inventory for the same products. You might also run into problems when many transactions try to update a single popular document, such as a leaderboard entry during a major gaming event. In some workflows, such as user authentication flows that frequently update session data, logs, and audit records, the same collections get touched over and over again, which can add up.

These situations can cause real slowdowns. Even quick transactions might have to wait in line for document locks, which means operations get processed one at a time instead of in parallel. The more contention there is, the more likely it is that write conflicts will happen, forcing transactions to retry.

In these cases, other approaches often work better. Instead of triggering lots of small transactions, you can batch updates in your application and send them as a single operation. For scenarios that need high throughput, a queue-based system can collect operations and process them in batches.

If you can’t avoid contention and need to update multiple documents in a transaction, try updating the most contended document first. That way, if the transaction needs to retry, less work will have to be repeated.

## Improper error handling and retry logic

Transactions can fail for a variety of reasons: transient network issues, temporary lock contention, or server-side conditions. If you’re using the Core API directly (rather than the Callback API, which handles retries automatically), it’s critical to implement proper error handling and retry logic in your application code.

One common mistake is failing to retry a transaction after encountering a TransientTransactionError, which is often recoverable with a simple retry. On the flip side, retrying too aggressively, without any delay or backoff, can make the situation worse by increasing contention. Another issue is retrying indefinitely without setting a maximum number of attempts, which can lead to wasted resources and stuck processes. Developers also sometimes mishandle UnknownTransactionCommitResult, retrying the entire transaction rather than just the commitTransaction command.

These kinds of mistakes can have serious consequences. Poorly managed retries can overload the server during high-traffic periods. Also, if retry logic isn’t idempotent or properly scoped, the same operation could be applied more than once.

To avoid these pitfalls, the best approach is to use the Callback API, which automatically retries transactions safely under the hood. If you’re using the Core API, make sure to implement retry logic with exponential backoff and jitter to avoid overwhelming the system while still allowing recoverable failures to succeed on a later attempt.

## Insufficient monitoring of transaction metrics

As the saying goes, “if you don’t measure it, you can’t improve it!” Failing to monitor transaction performance is a recipe for invisible problems turning into production outages.

To stay on top of transaction health, it’s important to track key metrics such as the number of active transactions and total transactions started, committed, and aborted. Monitoring transaction latency, that is, how long transactions take to complete, helps identify slowdowns. Additionally, keeping an eye on WiredTiger cache pressure and eviction rates gives insight into cache utilization. Lock-related metrics such as wait times and acquisition counts reveal whether transactions are being delayed by contention. Finally, tracking error rates, specifically how often transactions abort or require retries compared to total transactions, helps pinpoint instability.

For effective monitoring, set up dashboards in MongoDB Atlas or your preferred system to visualize these metrics. Create alerts for unusual trends such as spikes in abort rates or increasing transaction durations. Logging transaction execution details alongside database metrics can help correlate issues. Regularly reviewing these transaction patterns allows for ongoing optimization and early detection of potential problems.
