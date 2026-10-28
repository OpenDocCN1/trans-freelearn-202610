# Assessments

# Chapter 1

+   1. What are the three key components of event-driven architecture?
+   The key components of event-driven architecture are as follows:
+   - **Event producers**: These generate a stream of events
+   - **Event routers**: These manage event delivery between producers and consumers
+   - **Event consumers**: These listen to the events
+   For more details, see the *Understanding event-driven architecture section.*

+   2. What are the different Azure database services?
+   The different Azure database services are as follows: Azure SQL Database, Azure SQL Managed Instance, SQL Server on Azure Virtual Machines, Azure Database for PostgreSQL, Azure Database for MySQL, Azure Database for MariaDB, Azure Cache for Redis, Azure Database Migration Service, and Azure Managed Instance for Apache Cassandra.
+   For more details, see the *Exploring cloud databases section.*

# Chapter 3

+   - Which packages need to be added to read messages from a Service Bus queue using a .NET Core application?
+   `Azure.Messaging.ServiceBus.` For more details, see *Exercise 3 – publishing messages to a Service Bus queue using a .NET Core application.*

# Chapter 4

+   1. Do we need to add more packages for Durable Functions?
+   We need to add `Microsoft.Azure.WebJobs.Extensions.DurableTask`

+   2. What are durable functions in Azure?
+   Durable Functions is an extension of Azure Functions that lets you write stateful functions in a serverless compute environment. For more details, see the *Developing durable functions section.*