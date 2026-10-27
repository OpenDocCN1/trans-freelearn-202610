format. Poor initial data quality can dramatically impact the quality of a resulting graph, so Neo4j comes with some tricks to prevent bad-quality data from impacting graph construction and analysis. We can handle duplicates in Neo4j in a few ways, using MERGE and using CONSTRAINTS. In the following steps, we will be focusing on handling duplicate data with the MERGE function:

+   1. For this subsection, we assume that the Neo4j database is already populated with the GitHub social network. If not, follow the steps toward the start of this section to add data to Neo4j first.
+   2. In Chapters 5 to 8, we used the MERGE function in Neo4j, as opposed to the CREATE function, to prevent additional nodes from being created when reading in tabular data, or when executing write queries from Python. As discussed previously, the MERGE function first runs a match query to find a specified node or pattern, and if it already exists, does not write an additional node to the graph.
+   3. Let’s run a basic query with MERGE to attempt to write a node that already exists to the graph:
+   4. When running this in the Neo4j Browser, the resulting output will show that no changes were made to the database. The node was matched so no write operation took place. To illustrate, let’s look at the difference when we run the CREATE function with the same node specification:
+   5. The output will show that one node has been added. If you wish, run a MATCH query with the same command to visually display the two identical nodes that are now present in our database:
+   6. While the behavior of CREATE compared to that of MERGE is fairly obvious, what is less immediately clear is that the matching part of MERGE occurs for the entire pattern specified. So, should you attempt to merge two nodes and a relationship into Neo4j, and all nodes, relationships, and their attributes in the pattern don’t already match an entire pattern in the graph, the whole pattern will be created.
+   7. Let’s demonstrate this by merging data we might find in an edgelist. Here, we try to merge in Developer nodes that already exist in the graph, alongside a FOLLOWS relationship between them that does not yet exist:

```
MERGE (d:Developer {githubId: '1'})
```

```
CREATE (d:Developer {githubId: '1'})
```

```
MATCH (d:Developer {githubId: '1'}) RETURN d
```

```
MERGE (d:Developer {githubId: '1'})-[:FOLLOWS]->(d2:Developer {githubId: '2'})
```

The Neo4j output shows that some changes have been made to the graph. While the intention may have been to find nodes with githubId of 1 and 2 and add a relationship, because this relationship did not yet exist, two duplicate nodes with an edge between them have now been created.