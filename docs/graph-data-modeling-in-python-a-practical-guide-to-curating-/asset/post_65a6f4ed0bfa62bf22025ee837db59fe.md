
3. With our session set up, we are now ready to query the database. For this example, we will add the query parameter to be used to create the graph relationship to our Neo4j session:

```python
query = 'CREATE (:Person {name: "Jeremy"})-[:FOLLOWS]->(:Person {name:"Mark"})'
result = session.run(query)
driver.close()
```

These lines of code won’t return anything, since we have performed a write operation on the Neo4j database. However, we can confirm that it was successful by either opening the Neo4j Browser or using a read query from Python. For this first query, we will demonstrate both, beginning with the browser.

4. Go back to the Neo4j Browser for our Python DB database and run the following query from the command line at the top of the window:

```python
MATCH (n) RETURN n
```

This query essentially matches every node in the database and returns it for viewing. The nodes and relationships we added in Python will be displayed:

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_113_0.png)

Figure 5.2 – Neo4j Browser result

5. Alternatively, we can write read queries in Python to confirm that our write operation ran as intended. To establish a session, we will use the same code framework as for the previous write query:

```python
driver = GraphDatabase.driver(uri='bolt://localhost:7687',
                            auth=('admin', 'testpython'))

session = driver.session()
```

6. Now, instead of writing to the graph database, let's define two read queries. One query will match all Person nodes in the graph, while the other will match all FOLLOWS edges:

```python
node_query = 'MATCH (n:Person) RETURN n'
edge_query = 'MATCH () -[e:FOLLOWS]->() RETURN e'
```

7. Now, we can run both queries, before closing the connection. Accessing the data() method of run() transforms the results into a more consumable format that can more easily be handled in Python:

```python
node_result = session.run(node_query).data()
edge_result = session.run(edge_query).data()
driver.close()
print(node_result)
print(edge_result)
```

Printing our results shows that we have two results for n, which each contains one of our added Person nodes. We also see that we found one FOLLOWS edge, confirming that our write process ran as intended.

At this point, you may notice that to interface Python with Neo4j, we will be running these types of queries regularly. For this reason, we will create a reusable Python class to handle connections and queries to the database, removing the need to manually connect to the database every time we run a query.

8. Let's define a Python class named Neo4jConnect. The class will have a __init__ method, which will be called automatically when Neo4jConnect is instantiated. Because we will always want to connect to the database when we call Neo4jConnect, we can handle the driver connection here, using uri, user, and password parameters supplied to the class. We will also define a close() method in the class, to close the connection. A query() method will complete the class, which will take a Cypher query and execute it using the established connection, before returning the result:

```python
class Neo4jConnect:
    def __init__(self, uri, user, password):
        self.driver = GraphDatabase.driver(uri, auth=(user, password))
    def close(self):
        self.driver.close()
    def query(self, query):
        session = self.driver.session()
        result = session.run(query)
        return result
```

9. Let’s ensure that our class is functioning as intended by rerunning the read queries we executed previously. Now, we can create a connection object of the Neo4jConnect class, which handles authentication and the driver. Queries can now be run by accessing the query() method of the connection object, and the connection can be closed with the close() method:

```python
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')

node_cypher = 'MATCH (n:Person) RETURN n'
edge_cypher = 'MATCH ()-[e:FOLLOWS]->() RETURN e'

node_result = connection.query(node_cypher).data()
edge_result = connection.query(edge_cypher).data()

connection.close()
print(node_result)
print(edge_result)
```

Printing node_result and edge_result displays the same results as we obtained previously, showing that our class is implemented correctly. For the remainder of this chapter, we will continue to use the Neo4jConnect class to read and write to Neo4j.

Next, we will look at a practical use case for a graph database, and use Python and Cypher to query the database and perform complex analyses.

## Storing a graph in Neo4j

With our graph database set up, and our methods for interacting with Neo4j written, we can start to use Python and Neo4j to store and explore our graph data.

In this section, we will be looking at an air travel network between the US and Canada and analyzing its properties to find efficient routes between locations.

### Preprocessing data

To begin, let’s take a look at our data (sourced from Stanford University: https://snap.stanford.edu/data/reachability.html). We have two files, reachability_250.txt and reachability-meta.csv.

If we open reachability-meta.csv and take a look at the first few lines, we’ll find a list of information about cities in the US and Canada:

```text
"node_id","name","metro_pop","latitude","longitude"
0,"Abbotsford, BC",133497.0,49.051575,-122.328849
1,"Aberdeen, SD",40878.0,45.45909,-98.487324
2,"Abilene, TX",166416.0,32.449175,-99.741424
3,"Akron/Canton, OH",701456.0,40.79781,-81.371567
```

The file contains node attributes for our city nodes, keyed by the node ID. We have city names, the cities’ populations, and their respective latitudes and longitudes. This CSV file can be loaded into Neo4j as it is, so we don’t need to do any preprocessing.

However, our other file, reachability_250.txt, is not a CSV, which Neo4j prefers for imports. Opening reachability_250.txt and examining the first few lines, we can see that this contains a list of edges:

```text
# Directed graph: reachability.txt
# Transportation reachability of cities in the United States and Canada
# (edge from i to j if the estimated travel time from i to j is less than a threshold)
# Edges are weighted by similarity (more negative --> more dissimilar)
# Nodes: 456 Edges: 27677
# FromNodeId ToNodeId Weight
57 0 -84
113 0 -90
235 0 -170
```

According to the data source, the graph is directed. Each edge represents travel between two cities, represented by node IDs. The edge weighting is a measure of travel time between the two cities, where more negative values refer to further distances. There are also six lines of information that we will need to remove.

In the next steps, we will look at starting to clean and preprocess this data so that it is graph ready.

To preprocess this information ready for Neo4j import, let’s convert it to CSV format and remove the header lines. We can also make the distance measures non-negative to simplify the weightings. All of this preprocessing can be handled with the Python csv module and some list comprehensions, as we have done in previous chapters:

```python
import csv
with open('./data/reachability_250.txt', 'r') as txt:
    reader = csv.reader(txt, delimiter=' ')
    edges = [edge for edge in reader][6:]
    edges = [[edge[0], edge[1], int(edge[2])*-1] for edge in edges]

with open('./data/reachability.csv', 'w', newline='') as c:
    writer = csv.writer(c)
    for edge in edges:
        writer.writerow(edge)
```

This time, our data source is already in a sensible graph format, with a node attributes file and an edge list. The source also gives us information on a graph schema. The graph contains directed edges, and these edges are always between cities, making it a homogeneous graph. Our node attributes are the same types for each node, making the schema for this air travel graph fairly simple:

```text
node_id: int (unique)
name: str
population: int
latitude: float
longitude: float
```

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_117_0.png)

Figure 5.3 – US and Canada air travel graph schema

With our data in the correct format for Neo4j import and our air travel schema finalized, we can move on to creating the graph in the Neo4j database.

### Moving nodes, edges, and properties to Neo4j

We will be using Python to write to our Neo4j database. We will need to first write some Cypher queries that we will later get Python to send to Neo4j.

At this stage, we should also make sure that our Neo4j database is empty; if not, the MATCH (n) DETACH DELETE n query can be run in the Neo4j Browser window before starting.

Our Cypher scripts will read data from CSV files, which on a local installation of Neo4j should be in a specific folder that Neo4j can access. Follow these steps to get the data into Neo4j:

1. In the main Neo4j application window (not the Browser window for the current database), click the ... icon next to our newly created database.
2. From there, choose Open folder then Import, to open the /import folder in your operating system's file browser.
3. Copy reachability.csv and reachability-meta.csv to this import folder.
