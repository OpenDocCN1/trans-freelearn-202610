start to traverse through nodes and relationships held in the database, subsequent query performance is affected by cached data.

Let’s run a query to demonstrate this. The following query will search for developers in our GitHub social network that follow the repositories of at least 10 other developers. We do this by matching all relationships from each developer, d, to other developers, d2. Then, we use collect() to form a list of all the followed developers for each node and take its size. This is equal to the out degree for each node, so we name it as such. We limit the results to where out degree is at least 10, and return results ordered by this property:

```
MATCH (d:Developer)-[:FOLLOWS]-(d2:Developer)
WITH size(collect(d2)) as degree, d
WHERE degree >= 10
RETURN d.githubId, degree ORDER BY degree DESC
```

This represents a fairly exhaustive list of graph traversals across our nodes so that each node and relationship is considered in the initial MATCH statement. Try running this query once, make a note of its speed, and run it a few more times. Depending on the specifications of your machine, you should notice an increase in query speed in runs past the first execution.

This method of running a query on startup is known as warming the cache. Provided there is enough memory available, Neo4j will store as much information about the nodes and relationships it encounters during queries as it can fit into memory. When memory becomes limited, the oldest cached data will start to be replaced. We will examine this more closely in the next section.

# Memory limitations

When asking Neo4j to perform certain highly complex queries, it is likely that you will run into issues with available memory. If a particularly large or complex pattern is being matched, or a large amount of reads/writes are taking place in a single query, Neo4j may run out of memory in which to store intermediate information between query steps, resulting in dramatically slower query processing.

In extreme cases, it may run out of available memory altogether, resulting in canceled queries and Java error messages, such as the following:

```
java.lang.OutOfMemoryError: Java heap space
```

Running more simple queries to get around this issue isn’t always an option, so instead we can control the way Neo4j processes intermediate results by using the APOC library (https://neo4j.com/developer/neo4j-apoc/), and specifically, functions that are part of APOC.periodic. We first encountered this in Chapter 8, Perfect Projections, when working with a very large graph. To add the APOC plugin to our database, open the main Neo4j Desktop window, and select the database we have set up for this chapter, Common Issues DB. To the right side of the window, a menu will appear, from which you should select the Plugins tab. From this tab, choose APOC, and click Install.