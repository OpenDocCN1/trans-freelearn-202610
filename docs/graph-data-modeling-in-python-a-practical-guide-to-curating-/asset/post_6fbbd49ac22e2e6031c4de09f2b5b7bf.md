
- 4. Now that Neo4j can read the files, let’s write some Cypher to access their content. We can use `LOAD CSV WITH HEADERS` to load the `reachability-meta.csv` data, and the `as` function tells Neo4j to run the remaining code for each csv line, where each line is now named `row`. The headers are read in and can be used to access the corresponding csv columns, which we reference in `CREATE` to build nodes with the correct properties. Node IDs and population numbers are turned into integers with `toInteger()`, while our latitude and longitude attributes are converted into floats using `toFloat()`. When run from the Neo4j Browser, the following script will load the node attributes file:

```cypher
LOAD CSV WITH HEADERS from 'file:///reachability-meta.csv' as row
CREATE (city:City {
    node_id: toInteger(row.node_id),
    name: row.name,
    population: toInteger(row.metro_pop),
    latitude: toFloat(row.latitude),
    longitude: toFloat(row.longitude)
})
```

- 5. Next, we will use Cypher again to load in the edges file. Unlike our node properties file, `reachability.csv` doesn’t contain headers, so we use `LOAD CSV` to import the data into Neo4j. Our two `MATCH` functions find the corresponding nodes for our edge using slice notation (e.g., `[0]`) to refer to the csv columns. We can also use Cypher’s `toInteger()` function to convert our node attributes into integers, for both matching and merging. This identifies the nodes that we loaded in the previous step. Then, a `MERGE` step adds an `:AIR_TRAVEL` edge between these nodes, with `travel_time` added as an attribute from the third csv column:

```cypher
LOAD CSV from 'file:///reachability.csv' as row
MATCH (from:City {node_id: toInteger(row[0])})
MATCH (to:City {node_id: toInteger(row[1])})
MERGE (from)-[:AIR_TRAVEL {travel_time: toInteger(row[2])}]->(to)
```

- 6. With our data loaded into Neo4j, we should now confirm that it is as expected and try to access it from Python. To perform Cypher queries in Python, we will again be using the Neo4jConnect class we prepared earlier in this chapter. This can be imported from the custom package we have made for this chapter, namely `graphtastic` (apologies for the cheesy naming), in the GitHub repository (`https://github.com/PacktPublishing/Graph-Data-Modeling-in-Python/blob/main/CH05/`) and can be implemented in Python code as so:

```python
from graphtastic.database.neo4j import Neo4jConnect
```

- 7. Let’s write some simple Cypher queries inside Python to test that our data import was successful and that our graph can still be accessed. As before, we establish a connection with the Neo4jConnect class and run some Cypher using `connection.query()`. Our Cypher here simply matches all nodes, and count(n) aggregates those nodes into a single sum. We can then close the connection and print the result:

```python
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
cypher = 'MATCH (n) RETURN count(n)'
result = connection.query(cypher).data()
connection.close()
print(result)
```

The result should show that there are 456 nodes in our graph, which is the same number of rows in reachability-meta.csv minus one row for the header.

- 8. Next, let’s ensure that an edge we know should be present in Neo4j was added correctly. We can use the same pattern as in the node-counting example to run a Cypher query from Python, but this time with alternative Cypher code. The edge on the first line of reachability.csv is between node ID 57 and node ID 0. This time, for our test, let’s create a list of two elements to represent this edge. We will refer to these elements in our Cypher query:

```python
edge_test = [57, 0]
```

- 9. Now, we can write some Cypher as a Python string, which will accept attributes from edge_test using f-string replacement. We can use a MATCH function to match a whole edge, assigning n and m as our nodes and r as our relationship. Double curly braces are used around the node attributes we want to match, to escape Python’s f-string behavior in these areas. Where we do want replacement is where we reference the node_id attributes of each node, using values in edge_test. We can then add a RETURN statement to return the names on both nodes and the travel time between them, as attributes of the nodes and edge assigned to n, m, and r. Due to the amount of string modification going on here, let’s also print our Cypher query to see how Neo4j will receive it:

```python
cypher = f'MATCH (n:City {{node_id:{edge_test[0]}}})' \
    '-[r:AIR_TRAVEL]->' \
    f'(m:City {{node_id:{edge_test[1]}}})' \
    'RETURN n.name, m.name, r.travel_time'

print(cypher)
```

- 10. The f-string replacement and escaped characters will be resolved into a valid Cypher query, ready to be sent to Neo4j. We can send this query in the same manner as we have previously:

```python
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
result = connection.query(cypher).data()
connection.close()
print(result)
```

From the print statement, you should see that we matched one edge, from Calgary, AB, to Abbotsford, BC, with a travel time of 84. Try changing the node IDs in edge_test and rerunning to check for a few more edges present in reachability.csv.

At this point, you may be starting to see some of the power that comes from using Python and Cypher concurrently, where conditions and parameters in Cypher can be set dynamically through the use of f-strings, or other string replacement methods. Of course, this is useful not only in checking that data has been ingested correctly but also in downstream analysis in Python and pipeline construction. In the remainder of this chapter, we will use these methods to do some static analysis in both Cypher and igraph, using data streamed from Neo4j. We will be focusing on data ingestion and analytics pipelines involving both Neo4j and Python in Chapter 6, Pipeline Development.

## Optimizing travel with Python and Cypher

With our graph fully loaded into Neo4j, and our methods for querying data using Cypher and Python set up, we are ready to perform some more complex analysis. At the start of this section, we will use Cypher to answer questions and return answers in Python. Later, we will be doing more complex analysis, by sampling graph data from Neo4j and working with the sample in igraph.

Let’s begin by delving into the structure of our graph and asking some questions of our data to understand it better. The following steps will look at finding some relationships in the data:

- 1. The first query we will run will find out the highest population by city and we are going to return the name of the city and the city’s population as the result. ORDER BY will order by the population of those nodes (n). For those SQL people out there, these commands will look very familiar, and you will find the transition to Cypher much easier than those who have never used SQL. Anyway, back to the task at hand:

```python
cypher = 'MATCH (n) ' \
    'RETURN n.name, n.population ' \
    'ORDER BY n.population ' \
    'DESC LIMIT 1'
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
result = connection.query(cypher).data()
connection.close()
print(result)
```

- 2. Our results show us that the city with the highest population is New York. Another simple task we could ask of our graph is what cities we can travel from New York to in a relatively short travel time. For this, we can make use of the WHERE function, which again has a similar effect to its SQL counterpart. When used after a MATCH statement, we can filter the matched nodes by their travel time property with a WHERE function, before returning the names of the cities easily reachable from New York. As always, we then execute the query against the Neo4j database using Python:

```python
cypher = 'MATCH (n:City {name: "New York, NY"}) ' \
    'MATCH (n) -[r:AIR_TRAVEL]->(m) ' \
    'WHERE r.travel_time < 100 ' \
    'RETURN m.name'

connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
result = connection.query(cypher).data()
connection.close()

print(result)
print(len(result))
```

These first analyses are fairly simple and could be achieved using a relational database. Let’s now move on to some questions best tackled with a graph data structure and graph database, such as calculating paths.

Our graph contains flights available between cities in the US and Canada, but to make some journeys, connections will need to be made, involving stops along the way. As an example, let’s look at how we might travel from San Diego, CA, all the way north to St. John’s, NL.

- 3. We will begin, as many Cypher queries do, by using MATCH statements. First, we match the cities of interest, to find their corresponding nodes. Next, we match a path between these nodes using the [*..3] wildcard in place of specific edges or edge types. This notation allows paths to be matched up to a length of three hops from the starting node, n. We also assign these whole matched paths to the variable p. The last operation we need to do is return the paths, p, as well as the relationship details with travel time information with relationships (p), and map this to the rels variable:

```python
cypher = 'MATCH (n:City {name: "San Diego, CA"}) '\
    'MATCH (m:City {name: "St. John's, NL"}) '\
    'MATCH p=(n)-[*..3]->(m) ' \
    'RETURN p, relationships(p) as rels'

connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
result = connection.query(cypher).data()
connection.close()
```
