# Understanding MongoDB architecture

Now that you understand systems in general, it’s time we start looking at MongoDB systems in particular. Before we do that, let’s take a look at MongoDB architecture at a high level. We’ll dive deeper into each of these components throughout the book.

First, it’s important to understand the foundations of MongoDB. MongoDB was created to bridge the gap between traditional relational databases and simple key-value stores. The central idea was to bring together the best aspects of both worlds (flexibility, speed, and an easy-to-understand data model), while offering features such as powerful querying, indexing, and complex aggregations. In addition, from the early days, MongoDB had native libraries for many languages, built-in replication, horizontal scaling via sharding, and an intuitive document data model.

Key-value stores are often praised for their speed and simplicity. MongoDB, however, goes beyond that simple approach by letting you store and query entire documents (objects) with rich relationships and structure. Relational databases, on the other hand, organize data into tables with predefined columns and relationships and enforce strict schemas.

In the next section, we’ll take a closer look at the MongoDB document model and how it differs from a relational database.

# The document model: MongoDB’s foundation

In a relational database, data is typically normalized into separate tables to support a wide range of use cases. This can require complex joins for application-specific views. In contrast, MongoDB encourages you to model data as application-specific documents, optimized for how your application accesses and uses it. This flexibility allows for representing complex relationships and hierarchical data within a single document.

JavaScript Object Notation (JSON) is a lightweight, human-readable data format used to store and exchange data between applications. MongoDB’s document model stores data in JSON-like structures called Binary JSON (BSON) documents.