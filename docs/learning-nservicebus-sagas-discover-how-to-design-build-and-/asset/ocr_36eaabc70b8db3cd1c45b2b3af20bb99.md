+   - Availability of an assortment of common on-premise tools and services that work both on-site and in the cloud; for instance, using queuing and a SQL server that works locally and in the cloud.
+   - Development of websites quickly in the cloud through multiple tools, such as WebMatrix.
+   - Availability of on-demand services in using only what you need, and Microsoft tools to help report your resources and utilization for billing verification.
+   - Ability to move to other cloud services as your software model progresses and your needs change. Many business software applications that are developed in Azure can be used in other cloud services.
+   - Availability of simple data storage models in reporting and configuration of large data resources.

All of these features sum up that Microsoft allows you to develop systems on-premise or in the cloud using Microsoft tools. It does not force you to stay in the cloud or develop locally, but allows you to develop the pieces in the cloud that you see fit, and remove them from the cloud when you see fit. Many other cloud vendors may force you to use specialized tools to create solutions, which when you take them out of the cloud, will have to be completely rewritten to put into a different cloud service.

# Introducing Azure Storage Services

For data management, we can create a SQL Azure database in the cloud or a SQL server on-premise, or a hybrid thereof. An easier method to keep SQL servers out of the equation is simply to call Storage Services directly, which provides rows that directly map to data. Azure Storage Services can be used both on-site through the use of the Azure SDK, and in the Azure cloud.

Azure Storage can be used to store saga data in the tables, timeout persistence in the tables, subscription information in the tables, and to transport messages in the queues.

Other than providing SQL Azure for storing data in the cloud, Azure also provides the following other types of storage, of which there are three basic types:

+   - **Tables**: This is based on a key-value NoSQL table format.
+   - **Binary Large Object (BLOB)**: This is used for binary storage, such as video files.
+   - **Queues**: This is used for storing messages.