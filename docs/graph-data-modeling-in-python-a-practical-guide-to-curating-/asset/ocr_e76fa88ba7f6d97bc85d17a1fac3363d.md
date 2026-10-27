```
def query(self, query):
    session = self.driver.session()
    result = session.run(query)
    return result
```

9. Let’s ensure that our class is functioning as intended by rerunning the read queries we executed previously. Now, we can create a connection object of the Neo4jConnection class, which handles authentication and the driver. Queries can now be run by accessing the query() method of the connection object, and the connection can be closed with the close() method:

```
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

# Storing a graph in Neo4j

With our graph database set up, and our methods for interacting with Neo4j written, we can start to use Python and Neo4j to store and explore our graph data.

In this section, we will be looking at an air travel network between the US and Canada and analyzing its properties to find efficient routes between locations.

# Preprocessing data

To begin, let’s take a look at our data (sourced from Stanford University: https://snap.stanford.edu/data/reachability.html). We have two files, reachability_250.txt and reachability-meta.csv.