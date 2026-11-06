+   - **Using a transaction for a single document update:** Wrapping a single document update in a transaction is unnecessary as single-document operations are already atomic in MongoDB. This redundant approach adds processing overhead without providing any additional data integrity benefits.
+   - **Using transactions instead of better schema design:** Another common mistake is using transactions when the data could be modeled to fit within a single document. Taking time to design your schema properly can eliminate many transaction requirements and significantly improve performance.
+   - **Splitting logically related data across collections:** Developers also frequently create separate documents for data that logically belongs together, then use transactions to update them atomically. This approach not only complicates your code but also introduces potential performance bottlenecks and increases the likelihood of transaction conflicts.
+   - **Ignoring single-document atomicity:** MongoDB’s single-document atomicity is a powerful feature; don’t ignore it! Using multi-document transactions for operations that could be handled with single-document updates adds unnecessary overhead and complexity.

**Tip**

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
```