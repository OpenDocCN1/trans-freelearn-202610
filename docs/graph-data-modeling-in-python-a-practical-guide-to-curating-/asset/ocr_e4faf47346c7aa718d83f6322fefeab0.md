all nodes, and count (n) aggregates those nodes into a single sum. We can then close the connection and print the result:

```
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
cypher = 'MATCH (n) RETURN count(n)'
result = connection.query(cypher).data()
connection.close()
print(result)
```

The result should show that there are 456 nodes in our graph, which is the same number of rows in reachability-meta.csv minus one row for the header.

8. Next, let’s ensure that an edge we know should be present in Neo4j was added correctly. We can use the same pattern as in the node-counting example to run a Cypher query from Python, but this time with alternative Cypher code. The edge on the first line of reachability.csv is between node ID 57 and node ID 0. This time, for our test, let’s create a list of two elements to represent this edge. We will refer to these elements in our Cypher query:

```
edge_test = [57, 0]
```

9. Now, we can write some Cypher as a Python string, which will accept attributes from edge_test using f-string replacement. We can use a MATCH function to match a whole edge, assigning n and m as our nodes and r as our relationship. Double curly braces are used around the node attributes we want to match, to escape Python’s f-string behavior in these areas. Where we do want replacement is where we reference the node_id attributes of each node, using values in edge_test. We can then add a RETURN statement to return the names on both nodes and the travel time between them, as attributes of the nodes and edge assigned to n, m, and r. Due to the amount of string modification going on here, let’s also print our Cypher query to see how Neo4j will receive it:

```
cypher = f'MATCH (n:City {{node_id:{edge_test[0]}}})' \
    '-[r:AIR_TRAVEL]->' \
    f'(m:City {{node_id:{edge_test[1]}}})' \
    'RETURN n.name, m.name, r.travel_time'

print(cypher)
```

10. The f-string replacement and escaped characters will be resolved into a valid Cypher query, ready to be sent to Neo4j. We can send this query in the same manner as we have previously:

```
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
result = connection.query(cypher).data()
connection.close()
print(result)
```