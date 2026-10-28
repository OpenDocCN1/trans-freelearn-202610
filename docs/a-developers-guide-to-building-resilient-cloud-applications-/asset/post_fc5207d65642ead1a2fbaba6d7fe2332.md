
4. We will select whether we want to apply the Azure Cosmos DB free tier discount. We will get the first 1,000 RU/s and 25 GB of storage for free after creating this account.

We can only create one free tier Azure Cosmos DB account per Azure subscription. If we are already using it, this option will not be displayed when we create a new Cosmos DB account.

5. It is recommended to select **Limit the total amount of throughput that can be provisioned on this account** to prevent any unexpected charges related to provisioned throughput.

6. In the **Global Distribution** tab, we can enable geo-redundancy and multi-region writes:

A. Geo-redundancy is the ability to add more regions to your account

B. The **Georedundancy writes** capability allows you to use your provisioned throughput for databases and containers around the world

7. On the **Networking** tab, select a connectivity method to connect the Cosmos DB account to a public endpoint via public IP addresses, a private endpoint, or to all networks.

8. We also have the **Backup Policy** tab, **Encryption** tab, and **Tags** tab. You can update them according to your needs or keep their defaults as they are.

9. After selecting **Create**, select **Review + create**.

Once the deployment has been completed, you can check the Azure Cosmos DB account page and start exploring the different elements to use it.

With that, we have created an Azure Cosmos DB account by using the Azure portal. Azure Cosmos DB includes multiple APIs. In the next section, we will start exploring the Cosmos DB SQL API.

## Exploring the Cosmos DB SQL API

Azure Cosmos DB supports multiple APIs models, which makes it possible for you to select the ideal API for your application. In this section, we will specifically drill down into this SQL API. The Cosmos DB SQL API is the default Azure Cosmos DB API. You can store data using JSON documents to represent your data. Even though we are using the SQL API, we will still get to take advantage of all of the core features of Cosmos DB that are universal across all APIs.

The Core API for Cosmos DB is recommended when you’re building new solutions. This API uses SQL as a query language, along with JavaScript as its programming language. Both of these languages are pretty universal and popular, making it more likely that you already have experience with both.

Talking about the SQL query language specifically, you can write queries against Cosmos DB using the same SQL syntax you would use with products such as Microsoft SQL Server. Even though you are using the SQL query language, you can use these queries with your JSON documents, so you don’t have to do anything special to make your queries work with JSON. You can embed JSON objects directly into your queries. If you take a look at some unusual scenarios, you need to shape your JSON documents and test specific schema for the query result.

JavaScript is the language used by Cosmos DB for server-side programming. Storage procedures, triggers, and user functions are all written using JavaScript. Since JavaScript can natively process JSON objects, you'll find that JavaScript is ideal for manipulating properties and documents throughout your server-side code.

## Exercise 2 – creating an Azure Cosmos DB SQL API account

We will create an Azure Cosmos DB account by following the same steps in the first exercise, but we will select the Core (SQL) API option.

We will start by adding a new database and a container. After, we will add data to the created database. We will query the data and, in the end, use the SDK of Cosmos DB to connect to the Azure Cosmos DB SQL API while using C# as the programming language.

### Adding a new database and a new container

Let's look at the steps:

- 1. Select Data Explorer to create a new container, then select New container:

![](img/66aceec30d3e7dead2f71bb68a887688_213_0.png)

We need to fill in all the information needed:

- - **Database id** is the name of the database; we can add any unique name or we can use an existing one.
- - Check **Share throughput across containers**. This allows you to distribute your database’s provisioned throughput across all containers in your database. We are using this option to save costs.
- - Regarding **Database throughput (400 - unlimited RU/s)**, we can select **Autoscale** or **Manual**. We will leave the throughput at **400** required RU/s:

![](img/66aceec30d3e7dead2f71bb68a887688_214_0.png)

Figure 9.9 – Creating a new container – step 1

- 2. The second step is related to the container and the partition key. We will add the following information:

### * Container id ①

appointments

### * Indexing

Automatic   Off

All properties in your documents will be indexed by default for flexible and efficient queries. Learn more

### * Partition key ①

For small workloads, the item ID is a suitable choice for the partition key.

/appointment

### * Unique keys ①

+ Add unique key

### * Analytical store ①

On   Off

Azure Synapse Link is required for creating an analytical store container. Enable Synapse Link for this Cosmos DB account.

Learn more

Enable

Figure 9.10 – Creating a new container – step 2

The container ID is the name of your container, and the partition key is used to automatically distribute data across partitions to ensure scalability.

3. Click on OK to confirm the creation of a new container. Once one has been created, Data Explorer will display it and the new database:

![](img/66aceec30d3e7dead2f71bb68a887688_216_0.png)

Figure 9.11 – New database and new container in Data Explorer

Now, we’ll add data to our database. To do that, we will use JSON content; we will see how in the next section.

### Adding data to a database

We will select Items under appointments and use the following structure to add data to the document:

```
{
  "id": "1",
  "appointment": "appointment with a dentist",
  "patientname": "Hamida Rebai",
  "doctorname": "Rayen Trabelsi",
}
```

```
"isConfirmed": false,
"date": "2023-03-09T22:18:26.625Z"
}
```

On the right of the Documents pane, add the JSON content and click on Save. You can add more than one item by clicking on New Item:

![](img/66aceec30d3e7dead2f71bb68a887688_217_0.png)

Figure 9.12 – Adding data to your database using JSON content

Note that we don't have a schema for the data added in Azure Cosmos DB.

We have added data to our database using JSON content. We are now able to query the data.

### Querying data

We will use queries in Data Explorer to search for and filter data according to a specific criterion.

We will select New SQL Query at the top. We can add a default selection that will display all the data in our database:

```
SELECT * FROM c
```

In the next figure, we will select New SQL Query to add the SQL command and to execute it, we select Execute Query:

![](img/66aceec30d3e7dead2f71bb68a887688_218_0.png)

Figure 9.13 - Querying data from documents in Cosmos DB

We can add more filters to retrieve data. For example, if we need to order the result, we can use this query:

```
SELECT * FROM c ORDER BY c.id DESC
```

Figure 9.14 – Filtering data by order – DESC

You can use any SQL queries to search and retrieve data. You can also add stored procedures and triggers.

Data Explorer is an easy-to-use tool in the Azure portal that is used to add queries in SQL. We observe the results in JSON format, and we can measure the impact in terms of RU/s using the Query Stats tab.

```
SELECT * FROM c ORDER BY c.id DESC
```

![](img/66aceec30d3e7dead2f71bb68a887688_219_0.png)

We can generate metrics in a CSV file:

```
SELECT * FROM c ORDER BY c.id DESC
```

Results
Query Stats

| METRIC | VALUE |
|---|---|
| Request Charge | 2.96 RUs |
| Showing Results | 1 - 2 |
| Retrieved document count | 2 |
| Retrieved document size | 757 bytes |
| ↓ Per-partition query metrics (CSV) | |

Figure 9.15 – Query Stats in Data Explorer

In this section, we created a new Azure Cosmos DB container. We used the SQL API, and we added a new database and a new container. We added data to the database to test Data Explorer and retrieved the data using the SQL language.

In the next section, we will learn about the SDK, which we will use to connect to the Azure Cosmos DB SQL API using C# as the programming language.

## Exercise 3 – connecting to the Azure Cosmos DB SQL API with the SDK

In this section, we will explore Microsoft .NET SDL v3 for Azure Cosmos DB and explore the different methods and classes used to create different resources.

It is really simple to add the .NET SDK to your solution. Once we add the SDK to the project using NuGet by adding the Microsoft.Azure.Cosmos package, we will create a CosmosClient. We need an endpoint and a master key for a Cosmos DB account, which we will retrieve and add to the CosmosClient constructor. Now that the CosmosClient class has been configured, we simply call methods on it to do whatever we need. We can create databases and containers, query documents, and run stored procedures, which means that we can do all operations that we can do in Data Explorer.
