

![](img/782d45700e093b3d82b348c0ccc8ccba_291_0.png)

Figure 7.1 – Example of a columnar database

In a columnar database, not all columns need to be populated, and the number of columns for each row can be different as well. In the preceding example, just one row is shown:

- Key-value data stores: These databases store data as key-value pairs, with the key acting as an identifier and the value being the data that is being stored. The value is stored using an appropriate **hashing function**. Without going into a more in-depth discussion around hashing, which is outside the scope of this book, a hashing function simply converts an input of variable length to a compressed numerical fixed-length output using a mathematical function. An example of a key-value store is shown next:

| Keys | Values |
|---|---|
| 123 | ('Hi', 'Bob', '544-3434', 10000) |
| 343 | ('Hello', 'John', '434-9241', 5751) |
| 569 | ('Howdy', 'Tim', '979-3111', 50311) |

Figure 7.2 – Example of a key-value database

Key-value stores are optimized for querying data using keys and, unlike relational databases, they are not suitable for retrieving data using non-key values.

- Graph data stores: This type of data store manages entities, which is the data itself, but also the relationships between these entities. Entities are called **nodes**, whereas the relationships are called **edges**. This type of database places relatively more emphasis on how entities are connected to each other compared to relational databases. This type of database allows for extremely fast queries to understand complex entities and their relationships (nodes). Figure 7.3 shows an example of a graph data store:

![](img/782d45700e093b3d82b348c0ccc8ccba_292_0.png)

Figure 7.3 – An example of a graph database showing nodes and edges for travelers

Operations to access nodes and their edges are very efficient in graph data stores and are commonly used for use cases where large amounts of data exist and complex relationships need to be explored. For example, financial systems that are designed for fraud detection use graph data stores to analyze beyond the individual data points and form reasonable conclusions.

- Document data stores: These data stores are designed to store named string fields and values in an entity called a **document**. The format these documents use is JSON format and provides flexibility so that every stored record can be different than the other. Figure 7.4 shows an example of a document data store:

```
[
  {
    "year" : 2020,
    "make" : "Tesla",
    "model" : "Model S",
    "info" : {
      "Parts sourced from" : [ "Mexico", "China", "Brazil"],
      "First delivery date" : "2010-01-07T00:00:00Z",
      "Consumer product rating" : 7.2,
      "image_url" : "http://imageURL_Models"
    }
  },
  {
    "year" : 2021,
    "make" : "Lucid",
    "model" : "Air",
    "info" : {
      "Parts sourced from" : [ "Mexico", "China"],
      "First delivery date" : "2021-03-07T00:00:00Z",
      "Consumer product rating" : 8.1,
      "image_url" : "http://imageURL_LucidAir"
    }
  }
]
```

Figure 7.4 – A sample document store for electric vehicles

These types of data stores are suited for use cases where the rigid structure that a relational database provides is not very important. Rather, the flexibility to store and retrieve data as documents is preferred, such as in the case of high-traffic websites that allow users to create profiles.

Whether it's a relational or non-relational database, each has its own strengths and weaknesses that would make it appropriate for certain use cases. Non-relational databases are suitable for storing large amounts of data with little structure. Because a minimal structure is required, they offer more flexibility to changing business conditions. Where more structure, complex business logic, and ACID transactions are needed to ensure accuracy, completeness, and data integrity, a relational database is definitely a better architectural choice. The Salesforce platform uses relational databases as its foundational data store, but there are other services where it makes use of non-relational databases.

You may not have to play a role in deciding whether to use relational or non-relational databases, but understanding the idea behind them enables us to interact with them in a more meaningful way since we understand their strengths and weaknesses. This is especially applicable since most of us, if not all of us, use cloud-based services in one way or another. Now that we have a better appreciation of the different types of databases and understand the critical role they play in providing the services that we use, let's dive into LDVs.

## Large Data Volumes (LDVs)

LDVs in Salesforce don't have a strict formal definition, and if you ask an experienced architect what constitutes an LDV scenario, more than likely the answer you will get will be, It depends. The reason is that you could have an LDV scenario even with a much lower number of records than the general guideline that Salesforce provides. The general guideline is that once you have more than 1 million records in any object, you have an LDV scenario and you may start seeing the implications of LDVs.

Although the number of records is a consideration in determining whether an LDV scenario exists, that is not the only consideration. There could be several other factors that could create an LDV scenario even when the number of records is less than the generally recommended threshold, which is more than 1 million records. These factors include the following:

- **Transaction volume and complexity:** This is related to how much data is moving in and out of the organization. If you have integrations that are syncing data with external systems such as an ERP, and hundreds and thousands of records are being transferred into the system, that may not meet the recommended guidelines of an LDV scenario. But let’s say you have three of these integrations that run at the same time – now you would have exceeded the general guideline limits of an LDV.

  Another aspect is the complexity of the transactions. An integration that brings in and updates account records from an ERP is going to run much quicker than an integration that brings in product data from the ERP and forms bundles on the fly in Salesforce CPQ. Similarly, when data is being transferred out of Salesforce, a simple export of opportunity data is going to be much quicker than an integration that updates the ERP with contract, order, or quote data from Salesforce.

- **Data skew:** We will discuss the different types of data skews and why they impact performance in a later section.

- **Org complexity:** The complexity of the org in terms of numbers, the complexity of sharing rules, and the role hierarchy can have an impact and create an LDV scenario. Consider a scenario where, when records are inserted into the Account object, there are no sharing rules versus when an integration runs, and the org has complex Apex sharing rules. In the second scenario, each time the record is inserted, record visibility has to be determined, which can cause issues when a large number of records are being processed.

The number of records by itself is obvious, but also a combination of the factors we discussed here will result in an LDV scenario. This should highlight the importance of properly analyzing your org before finalizing an integration or migration approach. Failure to do so can result in issues down the road that will be both costly and time-consuming to fix. Let’s understand the implications of LDVs if they are not properly accounted for in the architecture.

### Knowing the implications of LDVs

You will likely see the effects of LDVs assuming they were not properly accounted for in the architecture. Here are some of the critical impacts when a system is not optimally architected considering LDV scenarios. This is one of the reasons why, when you are implementing enhancements or new applications in Salesforce, it is critical to ask questions, understand the answers, and design things based on the anticipated growth of data for the next 3–5 years.

- Reports and list views: These will most likely be the first things you hear from your users when you have hit an LDV scenario. Reports and list views will start to slow down, and users will complain about how a report used to run in a few seconds and now it takes forever to run it. Worse yet, these may start to time out because of the volume of data that the database has to sift through to return results.

- Search: Search results are going to take much longer if the search engine has to query millions of records. This is another area where users will quickly figure out that something is amiss and that it takes much longer than usual to return results. This is especially evident if a custom UI is being used for a search and the search engine has to query multiple objects and multiple wildcards are used.

- SOQL: Lightning components or Visualforce pages that use SOQL to return results will start to slow down or run into query timeout issues when the query has been running for more than 120 seconds. Regardless of whether the query runs for so long, you should always write optimized SOQL queries that return results quickly without having the user wait for a very long time.

