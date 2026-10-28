3. Select any API option. We will have the same options on the **Create Azure Cosmos DB Account - Core (SQL)** page. Start with the **Basics** tab:

![](bbox=[0.151, 0.158, 0.875, 0.393])

Figure 9.5 – Project Details on the Basics tab

On the **Basics** tab, we have the project details, which include common general information related to any Azure services, such as a **Subscription** and selecting a resource group. Then, we can add the information needed for the instance details, such as the Cosmos DB account name, the location, and the capacity mode for your database operations. We have two different capacity modes to choose from:

+   - **Provisioned throughput:** You can configure the amount of throughput you expect the database to deliver, but you are required to plan the best performance level for the application’s needs. This mode is expressed in **request units per second (RU/s)**. There are two types: *standard (manual)* and *autoscale*. An Azure Cosmos DB SQL API database is a schema-agnostic set of container management units. Each container is a scalable unit of throughput and storage. Throughput can be provisioned at the database level, container level, or both. Throughput provisioned to a database is shared by all containers within the database. All containers share throughput resources, so we may not get predictable performance for certain containers in our database.