
such as **Cypher**, **GraphQL**, or **Gremlin**. In this chapter, we will be using some basic Cypher to read and write data to a graph database. Neo4j has an extensive list of Cypher queries and their uses; see https://neo4j.com/developer/cypher/.

It is worth noting that while these query languages are powerful in terms of accessing and writing data, they do not currently offer the breadth of analyses and algorithms that a package such as *igraph* offers. In this chapter, we will be interfacing Python with a graph database, to demonstrate how *igraph* might be used in a complex analysis pipeline.

## Neo4j as a graph database

The graph database we will be using here is Neo4j, a commonly used high-performance database with a free local version. Neo4j can be queried using Cypher, which, of the graph query languages, is arguably the easiest for new users and so perfect for this introduction. For a more in-depth understanding of the usage of Cypher, see the documentation: https://neo4j.com/developer/cypher/.

Neo4j has analogous concepts to all of the graph concepts we have come across in previous chapters. In the Cypher documentation, while nodes are still named nodes, edges are more often referred to as relationships, and node and edge attributes are known as properties.

We have encountered several graphs with multiple types of nodes in heterogeneous graphs, and in Neo4j, these types of nodes have distinct *labels*. Labels are used heavily in the Cypher query language to access particular nodes and patterns; we will return to this concept when reading and writing to the graph database.

To get up and running with Neo4j, we need to carry out the following steps:

- 1. Neo4j can be installed using the instructions in the *Technical requirements* section of this chapter. This is shown in the following screenshot:

  ![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_109_0.png)

  Figure 5.1 – Neo4j database setup

- 2. Click **Start** on the new database to get it running, and the example database will be stopped, as Neo4j only supports one concurrent database running in the Desktop edition. The example database can now be deleted.

- 3. The Desktop edition of Neo4j comes bundled with the Neo4j Browser. Click **Open** next to our newly created database to open a Neo4j Browser window. Primarily, we will be focusing on using Neo4j for its function as a database, over its ability to visually display networks. From here, we can also create a new user, whose credentials we will use to access the database from Python. In the command line at the top of the window, enter the following:

  ```
  :server user add
  ```

- 4. A window containing fields to complete for a new user will appear. In the username field, add admin. For the password, again, we will use testpython. For roles, select both **admin** and **PUBLIC** from the drop-down menu, so that the new user will have full access to the database, both read and write.

- 5. Finally, click **Add User** to create our new admin user.

Now, with Neo4j installed and our Neo4j database created and configured, we will next need to learn how to interact with it, through the Cypher query language.

## The Cypher query language

Cypher is an open source query language compatible with several graph databases, including Neo4j. It shares some similarities with more common query languages, such as SQL, but is designed with graphs in mind, as opposed to tabular data. Much of Cypher's design is centered around matching node and edge patterns in a graph, using a special syntax that almost looks like drawing network paths.

When we want to refer to a node using Cypher, we enclose it in parentheses. Similarly, to denote a specific edge, square brackets are used. Nodes and edges are linked with dashes and arrows, like so:

```
(node) - [edge] -> (node)
```

This pattern would match nodes in a graph that are connected to other nodes, in a directed fashion. In practice, we are likely to be much more specific about the nodes we create or return.

We can begin by executing some simple Cypher queries in the Neo4j Browser window, before we start to query Neo4j directly from Python. Let's start by creating some nodes:

- 1. The CREATE function can be used at the start of a line to instruct Neo4j to create new nodes. To create a new node of the Person type with the name Jeremy, we open parentheses and use a colon to designate the type of the node. This is followed by opening curly braces to add the name attribute to this node, with the value Jeremy. We can follow this pattern to create two Person nodes:

  ```
  CREATE (:Person {name: "Jeremy"})
  CREATE (:Person {name: "Mark"})
  ```

- 2. These nodes have now been added to the database but are currently disconnected. We can connect these nodes by searching for them in the database with the MATCH function, followed by another CREATE function. This time, we are supplying temporary variables to our matched nodes by adding Jeremy and Mark before the colon in each MATCH. Then, we create a new edge between Jeremy and Mark by referring to their variables in parentheses, with a - [:FOLLOWS] -> relationship to denote that Jeremy follows Mark on a social media platform:

  ```
  MATCH (jeremy:Person {name: "Jeremy"})
  MATCH (mark:Person {name: "Mark"})
  CREATE (jeremy)-[:FOLLOWS]->(mark)
  ```

- 3. When dealing with a large amount of data, we may get duplicate information or need to update a database rather than create new items. The MERGE function exists to handle these scenarios and combines the functions of both MATCH and CREATE. When MERGE is used, a match takes place to check whether the pattern specified already exists in the graph database. If there is a match, no write operation takes place. If there is no exact match, the pattern is used in a CREATE function to add to the graph. The following line will have no effect, since a Person node with the name Jeremy already exists:

  ```
  MERGE (:Person {name: "Jeremy"})
  ```

- 4. However, if we run this MERGE function call, we will create a new Person node named Sophie, because the node is not already present in the graph:

  ```
  MERGE (:Person {name: "Sophie"})
  ```

The MERGE function also works with edges and longer path patterns.

- 5. We have covered the basics of writing to a Neo4j database, but let’s briefly look at how to retrieve information from the graph with read operations. We have already used MATCH, and we can continue to use it to find nodes or edges that match certain patterns and features to report back on. Then, we can use RETURN to retrieve that information. The following code will find all Person nodes and return them to the Neo4j Browser window:

  ```
  MATCH (n: Person)
  RETURN n
  ```

- 6. As well as accessing whole nodes, we can access node or edge attributes using ., as you would access an attribute of a Python object. To return just the name attribute of the nodes in our graph, we can run the following:

  ```
  MATCH (n: Person)
  RETURN n.name
  ```

- 7. Cypher also has methods for the basic analysis of results, including aggregations such as count(), sum(), and max(). For example, we can count and return the number of nodes in our database as follows:

  ```
  MATCH (n)
  RETURN count(n)
  ```

- 8. Finally, we can remove data from our database using DETACH DELETE. Here, we match all nodes in the database and remove edges attached to them, before deleting the nodes themselves:

  ```
  MATCH (n) DETACH DELETE n
  ```

Neo4j can be used in this manner to create nodes and relationships, as well as querying the database and examining parts of the graph. However, typically, in real-world cases, read and write operations need to be triggered by an external process, rather than typed directly into the Neo4j Browser window. In addition, visualization becomes less useful, as graphs stored in databases become large and cannot display every node and edge. With practicality in mind, for the rest of this chapter, we will mostly be interacting with the Neo4j database from Python.

## Querying Neo4j from Python

With Neo4j installed, we are ready to connect to the database through Python. To do this, we will be using the neo4j Python module, which contains everything we need to connect to the database and send queries to it from a Python script. We will iterate through this in the following steps:

- 1. Let’s start by creating a driver object for the Python-to-Neo4j connection. We will first need to import the GraphDatabase class from the neo4j module. Then, to create the driver, we will need to supply the uri, username, and password parameters to the GraphDatabase.driver() function:

  ```
  from neo4j import GraphDatabase
  driver = GraphDatabase.driver(uri='bolt://localhost:7687',
      auth=('admin', 'testpython'))
  ```

  Our uri parameter is the address of our local running instance of Neo4j. If the Neo4j database is being hosted on a separate server, then this would be substituted for the appropriate Internet Protocol (IP) address and port. Our username and password for the database we set up have also been passed as a tuple into the auth parameter.

- 2. Now, we need to create a session in order to send queries to the Neo4j database, using the session() method of our driver object:

  ```
  session = driver.session()
  ```
