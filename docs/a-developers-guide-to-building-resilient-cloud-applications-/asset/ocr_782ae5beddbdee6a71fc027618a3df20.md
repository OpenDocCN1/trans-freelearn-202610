```
var iterator = container.GetItemQueryIterator<dynamic
>(mysqlquery);
    var page = await iterator.ReadNextAsync();

    foreach (var item in page)
    {
        Console.WriteLine(item);
    }
}
```

We will debug the solution to display the different items. You can display the information that you need inside foreach:

![](bbox=[0.14, 0.35, 0.87, 0.55])

Figure 9.19 – Getting items using CosmosClient and a SELECT query

If we want to use LINQ instead of a SQL query, it is simple: you need to add a class that will include your object. In our case, I added the Appointment class; we will replace the code with the following code:

```
using (var client = new CosmosClient(endpointuri, primarykey))
{
    var container = client.GetContainer("HealthcareContainer",
    "appointments");
    var q = from d in container.GetItemLinqQueryable
    <Appointment>(allowSynchronousQueryExecution:true)
    select d;

    var documents = q.ToList();
    foreach(var document in documents)
```