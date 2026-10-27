
nodes in this graph, this is an amalgamation of both products a customer has purchased and their address information.

In addition to this, with a high number of node and relationship types in a graph data model, asking certain types of questions in an efficient way can be difficult. Consider how we would relate address information to brands of purchased products; we would have to traverse three relationships to go from Address nodes to Brand nodes:

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_182_0.png)

Figure 8.3 – Heterogeneous customer data graph

Instead, here we might consider using a graph projection of the customer data in order to answer a particular question, especially if this is a question we might ask of a graph database regularly, for example, as part of a recommendation system.

To answer questions on how brands relate to addresses of customers, we could collapse paths between these two node types into a single edge. In doing this, we lose information about the way addresses relate to brands through customers and products, but we hugely increase the efficiency of answering questions about how these node types relate by drastically reducing the number of hops required to traverse between these nodes. We can now easily compute the number of brands associated with an address, or perhaps locations of the world associated with purchasing particular brands of products.

These brands that are associated to distinct addresses can be seen in the following diagram, with an established HAS_PURCHASE_FROM edge-to-node relationship:

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_183_0.png)

Figure 8.4 – Bipartite projection of complex heterogeneous customer data

Now that you have an idea of why we might use a projection, we’re going to put this idea into practice in the next section. We will be exploring a large dataset of film information, creating projections to answer some questions that would be tricky to answer in a heterogeneous knowledge graph, and using some tricks in Neo4j to process data in efficient ways.

## How to use a projection

The steps that follow will refer to setting up a Neo4j instance, creating a data store, and using Cypher to work with our projection. This will combine the skills we have learned in subsequent chapters and distill them all in one place:

- 1. Let’s first create a new Neo4j database again, as we did in the last few chapters, to store our film knowledge graph. We will call it Projection DB and use the password testpython again, before starting up the database.
- 2. As we have detailed previously in Chapters 5, 6, and 7, we will create a new admin user using :server user add (in the Neo4j Browser), with the name admin, the password testpython, and the PUBLIC and admin roles added. For more detailed information on how to do this, please refer back to any of the previous three chapters.
- 3. In this chapter, because our dataset is large, we are going to use LOAD CSV to import data into Neo4j. To do this, we must first move movie.csv into a folder Neo4j can access.

In the main Neo4j Desktop window, click the ... icon next to Projection DB, then select Open folder | Import. This will open the data import folder for this database in the file explorer, in which we should then copy movie.csv (this can be found in the supporting GitHub repository’s data folder).

4. We will also be using the Awesome Procedures on Cypher (APOC) plugin (https://neo4j.com/labs/apoc/4.1/installation/) for Neo4j in our data import step. To install this, go to the main Neo4j Desktop window, select the Projection DB database, and look on the right of the window for the Plugins tab. Click on this tab, and under APOC, click Install and Restart.

5. Once installed, restart the Projection DB database.

6. With all this set up, it’s time to think about adding data to our database. As always, so we know what we’re working with, let’s take a look at the first few rows of the movie.csv file we added to our database’s import folder:

```
002 Operazione Luna,COUNTRY,Italy
002 Operazione Luna,DIRECTOR,Lucio Fulci
002 Operazione Luna,STARRING,Ciccio Ingrassia
002 Operazione Luna,STARRING,Franco Franchi
002 Operazione Luna,STARRING,Linda Sini
```

Here, we see that movie.csv contains edge data, with the first and third columns containing the names of related entities. These entities, on inspection, can be different types of film-associated data. Each of these entities is related by the concept in the second row – COUNTRY, DIRECTOR, and so on. This type of data structure is often seen when creating graph databases, and each row is known as a triple.

7. This time, our dataset seems a lot larger than those we worked with previously. We should check some of the properties of our import data before diving into loading it to Neo4j. In Python, we can read in the dataset and check the number of rows first:

```
import csv
with open('./data/movie.csv', 'r') as c:
    reader = csv.reader(c)
    data = [line for line in reader]

print(len(data))
```

8. From our print statement, we see that movie.csv contains over 410,000 triples. We can also check how many types of edges we will be importing into the Neo4j database, by taking a set() of the second column:

```
edge_types = set([triple[1] for triple in data])
print(edge_types)
```

This shows that we have four different edge types in movie.csv: AWARD, DIRECTOR, COUNTRY, and STARRING. This means our node entities must correspond to awards, directors, countries, and actors, as well as the films they relate to. Now that we have a better understanding of what data we have access to, we can design an approach to Neo4j data ingestion.

followed by YIELD, which carries over an output of apoc.create.relationship. We can carry over the rel output, but we will not use it, as we are writing to the graph and don’t need to use any returned value. Similarly, our subquery requires that something is returned, but because we don’t need this, we can return a dummy variable, x:

```
:auto LOAD CSV FROM 'file:///movie.csv' AS row
CALL {
    WITH row
    MERGE (n:Entity {name:row[0]})
    MERGE (m:Entity {name:row[2]})
    WITH n, m, row[1] as type
    CALL apoc.create.relationship(n, type, {}, m)
    YIELD rel
    RETURN 1 as x
} IN TRANSACTIONS OF 1000 ROWS
RETURN x
```

This query will run in batches and may take up to a few minutes, depending on the machine the Neo4j database is running on. Once the data is loaded in, we can check that the data is structured as intended by running queries in the Neo4j Browser.

14. Let’s look for an edge we know should be present in the database, for example, the edge corresponding to the first row in movie.csv. This has two entities, a film, 002 Operazione Luna, and Italy, connected by a COUNTRY edge. We can match this edge using Cypher and return it to confirm it exists in Neo4j. Running this query, we should see the nodes and COUNTRY edge returned in the Neo4j Browser:

```
MATCH (n:Entity {name:'002 Operazione Luna'})-[:COUNTRY]-(m:Entity {name:'Italy'})
RETURN n, m
```

The country matching to the movie will be shown in the graph relationship diagram:

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_186_0.png)

Figure 8.5 – Entity matching on an edge

With our film dataset loaded into a persistent database, we can create various projections in order to answer different questions using our graph data. If we want to see what actors work together on various films, we could create a projection of co-starring actors. We could also create a projection to dive deeper into what films share common actors, and how much this is influenced by the country used in filming. We are next going to tackle how to generate both of these predictions, before moving on to using projections to sensibly and efficiently analyze our data.

## Creating a projection in igraph

Let’s create our first projection in Python, by reading from the Neo4j database and importing it into igraph. To do this, we will use a package we have created to import the functions we will use again and again in these graph projects. The function we will use is the Neo4jConnect function and this can be imported from the graphtastic package that is found in the associated GitHub repository (link in the Technical requirements section of this chapter). The following steps will cover the importing of this module to connect to Neo4j and then how you create a projection, step by step, with igraph and Python. Let’s get projecting:

- 1. Import the Neo4jConnect class from the graphtastic package:

```
from graphtastic.database.neo4j import Neo4jConnect
```

- 2. With our class and methods in place to connect to the Neo4j database, we can now figure out how to get some data from Neo4j, in the form of a graph projection. The projection we’re aiming for is a graph containing information on what actors co-star alongside each other in films. In our Neo4j database, we have the STARRING relationship, which connects film Entity nodes to actor Entity nodes.

- 3. Let’s quickly take a look at these relationships by looking up a pair of actors that starred in the same film, with some Cypher in the Neo4j Browser. Here, we can identify the correct types of Entity nodes by using MATCH with the specific STARRING relationship type:

```
MATCH (act1:Entity)<-[:STARRING]-(film:Entity)-[:STARRING]->(act2:Entity)
RETURN act1, film, act2 LIMIT 1
```

We can see from the browser results that actors are connected through films. However, in our projection, we want to connect co-starring actors. So, any Cypher query we run in Python to get edge data for our projection needs to find nodes and edges that have this pattern and return just the nodes containing actor information.

- 4. Knowing this, we can write a method in Python to access this actor data, get_co_stars_neo4j(). This method will take only a Neo4j connection object and use the pattern we found in a MATCH statement using the Neo4j Browser to find co-starring actors. We only need to return the node on the edge of our pattern, here aliased to act1 and act2. We execute the query using the connection.query() method, and access a list of dictionaries containing our query result with result.data(). As a last step, we clean up the data structure by accessing
