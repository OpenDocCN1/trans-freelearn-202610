```
private static readonly string PrimaryKey = "add your primary key";
```

This information can be found in Keys under Settings in the Azure Cosmos DB account. This is an example:

![](bbox=[0.15, 0.21, 0.87, 0.56])

Figure 9.17 – Getting the primary URI and primary key from Keys under Settings

+   3. If we want to create a new database and a new container, we can use this code snippet:

```
private CosmosClient cosmosClient;
private Database database;
private Container container;
private string databaseId = "appointmentDB";
private string containerId = "appointmentContainer";

// Runs the CreateDatabaseAsync method
await this.CreateDatabaseAsync();

// Run the CreateContainerAsync method
await this.CreateContainerAsync();
private async Task CreateDatabaseAsync()
```