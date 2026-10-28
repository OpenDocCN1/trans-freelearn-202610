2. On the Select API option page, we have to select an API to create a new account. Note that we are not able to change it after account creation:

Select API option ...

![](bbox=[0.13, 0.15, 0.85, 0.34])

Figure 9.4 – The Select API option page in Cosmos DB

We have these APIs to choose from:

+   - Core (SQL) is recommended for new applications. It is a native API used for documents. It's Azure Cosmos DB's core.
+   - Azure Cosmos DB API for MongoDB is a native SQL API with APIs for MongoDB. MongoDB is a document database. This API is recommended if you already have an existing MongoDB workload and you would like to migrate it to Azure, for example.
+   - Cassandra is a fully managed database service dedicated to Apache Cassandra apps. If you already have an existing Cassandra workload, you can use this API to migrate it to Azure. Cassandra is a distributed NoSQL database management system used for large data.
+   - Azure Table is also a fully managed database service for Azure Table Storage apps. If we decide to migrate an existing Azure Table Storage resource to Cosmos DB, we can use this API.
+   - Gremlin (Graph) is Apache TinkerPop's graph traversal language. Gremlin is a functional dataflow language that allows users to concisely express complex traversals (or queries) of an application's property graph. Cosmos DB is a fully managed graph database service that uses Gremlin. If we would like to add new workloads that need to store relationships between data, this option is recommended.