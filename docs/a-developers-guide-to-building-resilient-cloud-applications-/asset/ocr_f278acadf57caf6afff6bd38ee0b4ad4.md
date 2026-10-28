```
```csharp
{
    var d = document as dynamic;
    Console.WriteLine(d);
}
}
```
```

Azure Cosmos DB also provides language-integrated transactional execution of JavaScript, which allows you to write stored procedures or triggers, user-defined functions, and much more.

Azure Cosmos DB is a distributed database system designed for low latency, elastic throughput scalability, well-defined semantics for data consistency, and high availability. It enables data reading and data writing from a local replica of your database. Azure Cosmos DB replicates data across all regions associated with your Azure Cosmos DB account.

If we build an application that requires a fast response time anywhere in different regions, with unlimited elastic scalability of throughput and storage and high availability, we will use Azure Cosmos DB.

You have to configure your databases to be globally distributed and available in any Azure region to achieve high availability with Azure Cosmos DB. Azure Cosmos DB is a managed multi-tenant service. It manages all details of individual compute nodes transparently. So, users don’t have to worry about patching and planned maintenance. We also have replica outages, which refer to a single node failure in an Azure Cosmos DB cluster deployed in an Azure region. Azure Cosmos DB automatically mitigates replica failures by guaranteeing at least three replicas of your data in each Azure region of your account within a quorum of four replicas.

In the next section, we will connect an application’s ASP.NET Core Web API deployed on Azure App Service with Azure Cosmos DB as a database.

# Exercise 3 – connecting Azure App Service with Azure Cosmos DB

In this section, we will connect Azure App Service with Azure Cosmos DB as a database. An ASP.NET Web API will connect to a Cosmos DB database to manage the appointments booked by patients before sending them to the doctor’s dashboard.