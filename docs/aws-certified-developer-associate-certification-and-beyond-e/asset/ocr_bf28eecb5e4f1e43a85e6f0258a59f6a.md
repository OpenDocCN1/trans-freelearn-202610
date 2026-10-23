Another key aspect of DynamoDB is that it is schema-less. Every table must have a primary key much like traditional relational databases to uniquely identify a record. However, beyond this, there are no other constraints like those found in traditional SQL databases. DynamoDB can be used to host structured and semi-structured data including JSON documents. Furthermore, DynamoDB presents itself as a complete web service and you make calls to the database using HTTP(S) requests and responses. With DynamoDB, you can use either the DynamoDB API or PartiQL, a SQL-compatible query language, to add and retrieve data from a table.

Here is an example of a PartiQL for DynamoDB query:

```
```
SELECT AlbumTitle, Year, Price
FROM Music
WHERE Song='All Night Long'
```
```

Like relational databases, DynamoDB uses the concept of tables, which can contain items (records) that have attributes (values). A primary key is used to uniquely identify an item and you can also have secondary indexes to offer more querying options. You will look at these core components next.

# # DynamoDB Core Components

The following are the core components of a DynamoDB database:

+   * **Tables:** Data is stored in tables just like other relational databases. A table represents a collection of data. For example, the Employees table will contain all employee details, their salary information, pension contributions, etc.

+   * **Items:** Each table can then contain one or more items. This is a group of attributes that define the item and its characteristics. For example, in the Employees table, each employee would be represented as an item. Each item will need to have a primary key assigned to it that uniquely identifies that item.

+   * **Attributes:** Each item comprises one or more attributes. These attributes are used to store values associated with the item. For instance, examples of attributes in the Employees table are **first-name**, **last-name**, and **department**. These attributes help you define the values for each item in the table.