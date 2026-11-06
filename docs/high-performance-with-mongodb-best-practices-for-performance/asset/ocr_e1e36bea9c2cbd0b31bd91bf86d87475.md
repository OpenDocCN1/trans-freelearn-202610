Together, the ACID properties provide a foundation for predictable and reliable behavior in MongoDB transactions. They ensure that multi-step operations are either fully completed or have no effect, helping maintain data integrity even in the presence of concurrent workloads, application errors, or system failures. This can be particularly important for applications that coordinate changes across multiple collections, such as processing orders, updating inventory, or managing user accounts.

# Document-level atomicity versus multi-document transactions

Understanding the difference between MongoDB’s default document-level atomicity and multi-document transactions is crucial for effective data modeling and optimizing performance.

# Document-level atomicity

MongoDB has always guaranteed that operations on a single document are atomic. When you update multiple fields within a document, either all changes are applied or none are; there’s no in-between state where only some fields are updated. These changes are also only ever visible to other threads together or not at all. This built-in atomicity, combined with MongoDB’s rich document model that allows for embedding related data, means many use cases don’t actually need multi-document transactions at all.

For example, if you’re tracking an order with its line items, embedding the line items within the order document ensures that updates to both the order status and its items happen atomically:

```
```json
{
  "_id": "order123",
  "customer": "jane_doe",
  "status": "shipped",
  "items": [
    { "product": "widget", "quantity": 5, "price": 10 },
    { "product": "gadget", "quantity": 1, "price": 20 }
  ]
}
```
```

This approach keeps data tightly coupled and ensures consistency without the overhead of coordinating changes across multiple documents.