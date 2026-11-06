# Best practices when using frameworks with MongoDB

When leveraging application frameworks with MongoDB, consider these important practices:

+   - **MongoClient reuse:** A MongoClient instance maintains connection pools to all cluster members, as well as monitoring connections. It’s important to reuse this instance to prevent the unnecessary duplication of connections, which is commonly achieved via the Singleton pattern or proper configuration of the framework’s IoC container.
+   - **Efficient schema design:** Design your schemas to minimize redundancy while optimizing query performance. Use embedded documents and references appropriately to balance data normalization and denormalization based on application access patterns, as well as to avoid potential N+1 problems.
+   - **Avoiding N+1 problems:** Be mindful of N+1 query issues, where multiple separate queries are executed to fetch related data. Use techniques such as eager loading, the MongoDB aggregation pipeline, or batch operations to reduce database round-trips.
+   - **Caching strategies:** Implement appropriate caching layers to reduce database load and improve application performance. Most frameworks offer built-in caching solutions that can be configured to work with MongoDB query results.
+   - **Lazy versus eager loading:** Choose between lazy and eager loading based on your application’s specific needs. Lazy loading defers the loading of related data until it’s accessed, while eager loading fetches all related data upfront. Each approach has trade-offs in terms of performance and memory usage.
+   - **Life cycle callbacks:** Leverage the life cycle hooks provided by your framework’s ODM to execute custom logic at various points in a document’s life cycle. These callbacks allow you to implement consistent business rules, such as data validation, transformation, or triggering events.
+   - **Performance monitoring:** Instrument your framework code to monitor MongoDB operation performance. Understanding query patterns and execution times helps identify bottlenecks and optimization opportunities.

Application frameworks significantly enhance MongoDB development by providing structured, reusable components that streamline common tasks. Their integration with ODMs offers a natural development experience in which database operations align with idiomatic programming patterns. By selecting a framework that complements your language expertise and project requirements, development teams can build scalable, maintainable MongoDB applications more efficiently.

As we’ll explore in the next section, understanding the specific patterns and pitfalls for popular ODMs and frameworks will help you leverage MongoDB’s capabilities while maintaining clean, performant code.