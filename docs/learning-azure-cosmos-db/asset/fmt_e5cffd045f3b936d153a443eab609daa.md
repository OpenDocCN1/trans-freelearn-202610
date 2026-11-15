
# Querying Cosmos DB

In the last chapter, we learned about various data models and created databases and tables or collections in those data models. It's time to ingest some data in those data models and do the query to see how effective it is.

In this chapter, we are going to cover the following topics:

*   Performing CRUD operations in DocumentDB.
*   Performing CRUD operations in MongoDB.
*   Performing CRUD operations in Graph DB.
*   Performing CRUD operations in tables (key value store).
*   Performing CRUD operations in Cassandra.

Let's begin with Document DB queries.

## Performing CRUD operations in DocumentDB

DocumentDB is Microsoft's own NoSQL database with an SQL-like query feature. When I say SQL-like, I mean it allows us to query the JSON documents using our all-time favorite SQL queries.

Before beginning with the queries, let's learn the basics of DocumentDB query.

### DocumentDB query basics

Every DocumentDB query consists of a `SELECT` statement and optional `FROM` and `WHERE` clauses as per ANSI-SQL standards.

For each query, the source in the `FROM` clause is searched and traversed. Then the filter part in the `WHERE` clause is applied on the source to retrieve a subset of JSON documents.

At the end, the `SELECT` clause is used to populate the requested JSON values in the select list.

The order of the statements can be like the following:

```
SELECT <select_list> 
[FROM <from_specification>] 
[WHERE <filter_condition>]
[ORDER BY <sort_specification]  
```

Let's begin with the query.

### Creating data in Cosmos DB

To perform query operations, we need to add in some data. You can add as many documents as you want but, for the sake of example, I am going to add two nested JSON documents.

The first document is shown in the following code:

```
{ 
  "id": "100", 
  "lastName": "Doe", 
  "parents": [ 
     { "firstName": "John" }, 
     { "firstName": "Mary"} 
  ], 
  "children": [ 
     { 
         "firstName": "Emily",  
         "gender": "female",  
         "grade": 5, 
         "pets": [{ "givenName": "Fluffy" }] 
     } 
  ], 
  "address": { "state": "WA", "county": "King", "city": "seattle" }, 
  "creationDate": 1431620472, 
  "isRegistered": true 
} 
```

Here is the second document, with a few more changes:

```
{
   "id": "200",
   "parents": [
       { "familyName": "Wakefield", "givenName": "Robin" },
       { "familyName": "Miller", "givenName": "Ben" }
   ],
   "children": [
       {
         "familyName": "Merriam", 
         "givenName": "Jesse", 
         "gender": "female", "grade": 1,
         "pets": [
             { "givenName": "Goofy" },
             { "givenName": "Shadow" }
         ]
       },
       { 
         "familyName": "Miller", 
          "givenName": "Lisa", 
          "gender": "female", 
          "grade": 8 }
   ],
   "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
   "creationDate": 1431620462,
   "isRegistered": false
 }
```

To create these documents using Azure portal, perform these steps.

1.  Log in to your Azure portal and click on the **Azure Cosmos DB** option on the left side of the option bar.
2.  Go to **Data Explorer** from the left menu and select the collection we created in the last chapter.
3.  Click on the **New Document** button and add the JSON mentioned above.

Once the documents are created, we can query them.

### Reading data from DocumentDB

Let's query our data using SQL query structure:

1.  Click on the **New SQL Query** button and paste this query:

```
SELECT *  
FROM Families f  
WHERE f.id = "100" 
```

2.  This will yield the following result:

```
[{ 
  "id": "100", 
  "lastName": "Doe", 
  "parents": [ 
     { "firstName": "John" }, 
     { "firstName": "Mary"} 
  ], 
  "children": [ 
     { 
         "firstName": "Emily",  
         "gender": "female",  
         "grade": 5, 
         "pets": [{ "givenName": "Fluffy" }] 
     } 
  ], 
  "address": { "state": "WA", "county": "King", "city": "seattle" }, 
  "creationDate": 1431620472, 
  "isRegistered": true 
}] 
```

3.  You can also execute a traditional order by queries, similar to the following one:

```
SELECT c.givenName  
FROM Families f  
JOIN c IN f.children  
WHERE f.id = '200' 
ORDER BY f.address.city ASC
```

4.  This will yield the following result:

```
[ 
  { "givenName": "Jesse" },  
  { "givenName": "Lisa"} 
]
```

5.  You can also run the sub-queries, that is, get the subdocuments from the main document. You can refer to the following query:

```
SELECT *  
FROM Families.children 
```

6.  This will yield the following result:

```
[ 
  [ 
    { 
        "firstName": "Emily", 
        "gender": "female", 
        "grade": 5, 
        "pets": [ 
          { 
              "givenName": "Fluffy" 
          } 
        ] 
    } 
  ], 
  [ 
    { 
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", 
        "grade": 1 
    }, 
    { 
        "familyName": "Miller", 
        "givenName": "Lisa", 
        "gender": "female", 
        "grade": 8 
    } 
  ] 
] 
```

So, we now know how to create and query documents. Let's perform an `UPDATE` and `DELETE` operation.

### Updating and deleting documents

Azure portal does not allow the running of these queries, that is, updating and deleting the document using queries, but you can do it using SDKs of your choice or from the Azure portal UI.

To edit, you can just run the `SELECT` query and edit the document live and click on the **Save** button to make the changes take effect, or you can click on the **DELETE** button to remove the document.

We will look over updating and deleting using Azure SDK in [Chapter 5](#bbbd9fe4-f9da-44d6-9d1d-99612ef7197f.xhtml), *Integrating Node.js with Cosmos DB*.

Let's look over the MongoDB query operation.

## Performing CRUD operations in MongoDB

MongoDB is one of the popular NoSQL databases. You need to create a MongoDB database and collection and you can do basic CRUD using the Azure Terminal in the browser, or get a connection string and connect it to your node app.

1.  Open your Azure portal and create a new MongoDB instance; refer to the following screenshot for reference:

![](assets/ce361e5c-2669-4dd5-94dd-a34b4e79d0fd.png)

2.  Once it is created, you can add a new collection to it. Refer to the following screenshot for reference:

![](assets/db5a72b5-aa15-48eb-b86b-75e9a7332d6a.png)

3.  Now you need to retrieve a connection string. Go to the **Settings | Connection String** option and copy the connection string.

Keep it somewhere secure and do not share it. 

Now you can connect your Node.js application to this MongoDB instance using the connection string.

We are going to cover Node.js in [Chapter 5](#bbbd9fe4-f9da-44d6-9d1d-99612ef7197f.xhtml), *Integrating Node.js with Cosmos DB*.

You can, however, do the CRUD on the Azure browser terminal itself.

4.  Click on the **New document** button on top of the bar and add the following JSON to create a document:

```
{ 
    "id": "10", 
    "name": "Shahid", 
    "location": "Mumbai", 
    "job": "Engineer" 
} 
```

5.  Once a document is created, you can view it using the query, or by clicking on the left bar which contains lists of documents. You can delete it by clicking on that small cross button.

We will cover doing the CRUD using code in the next chapter.

Let's see how to do crud operations in Graph DB.

## Performing CRUD operations in Graph DB

As discussed in [Chapter 3](#2884078b-5f26-4689-a71d-230d1f20e997.xhtml), *Cosmos DB Multi-model Databases*, a graph data model is a structure that's comprised of vertices and edges. Both vertices and edges can have a set number of properties:

*   **Vertices** represent discrete objects, such as a person, a place, or an event.
*   **Edges** represent relationships between vertices. For example, a person might know another person, be involved in an event, and recently have been at a location.
