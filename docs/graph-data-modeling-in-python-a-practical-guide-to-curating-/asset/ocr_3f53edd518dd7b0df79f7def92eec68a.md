From the print statement, you should see that we matched one edge, from Calgary, AB, to Abbotsford, BC, with a travel time of 84. Try changing the node IDs in edge_test and rerunning to check for a few more edges present in reachability.csv.

At this point, you may be starting to see some of the power that comes from using Python and Cypher concurrently, where conditions and parameters in Cypher can be set dynamically through the use of f-strings, or other string replacement methods. Of course, this is useful not only in checking that data has been ingested correctly but also in downstream analysis in Python and pipeline construction. In the remainder of this chapter, we will use these methods to do some static analysis in both Cypher and igraph, using data streamed from Neo4j. We will be focusing on data ingestion and analytics pipelines involving both Neo4j and Python in Chapter 6, Pipeline Development.

# Optimizing travel with Python and Cypher

With our graph fully loaded into Neo4j, and our methods for querying data using Cypher and Python set up, we are ready to perform some more complex analysis. At the start of this section, we will use Cypher to answer questions and return answers in Python. Later, we will be doing more complex analysis, by sampling graph data from Neo4j and working with the sample in igraph.

Let’s begin by delving into the structure of our graph and asking some questions of our data to understand it better. The following steps will look at finding some relationships in the data:

+   1. The first query we will run will find out the highest population by city and we are going to return the name of the city and the city’s population as the result. ORDER BY will order by the population of those nodes (n). For those SQL people out there, these commands will look very familiar, and you will find the transition to Cypher much easier than those who have never used SQL. Anyway, back to the task at hand:

```
cypher = 'MATCH (n) ' \
    'RETURN n.name, n.population ' \
    'ORDER BY n.population ' \
    'DESC LIMIT 1'
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
result = connection.query(cypher).data()
connection.close()
print(result)
```