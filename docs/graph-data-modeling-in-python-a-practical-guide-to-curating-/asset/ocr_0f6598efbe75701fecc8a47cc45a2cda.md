In the next section, we have a second projection to analyze, which is complementary in some ways to our co-starring network.

The projection we previously set up in Neo4j represents films that share a common actor, so let’s dive into using our projection to gain insight into any interesting patterns present.

# Analyzing the Neo4j film projection

The projection we have in Neo4j contains `Film` nodes only, which share an edge if they have actors in common. Let’s begin, as we did in *igraph* for the co-stars projection, by characterizing some basic information about our graph.

The following steps will inform you of the Cypher queries you need to execute in the Neo4j Browser to get similar results to the Python analysis we have done in previous sections:

+   1. First, let’s count the number of nodes in the graph with a simple Cypher query in Neo4j Desktop, by using MATCH to find nodes of the `Film` type only, and returning the number of these nodes with the `count()` function:
+   
+      ```cypher
+      MATCH (:Film) RETURN count(f)
+      ```
+   
+      The console output shows us that we have 79,478 Film nodes in our projection. When creating the projection, we had to write a batched query to read and write our data, and around 80,000 nodes wouldn’t usually cause us any issues.

+   2. Let’s also check how many HAS_COMMON_ACTORS relationships we created when building our projection, by matching this relationship between our `Film` nodes with a pattern. This time, we alias the relationship to r, and again return the number of nodes with `count()`:
+   
+      ```cypher
+      MATCH (:Film)-[r:HAS_COMMON_ACTORS]->(:Film) RETURN count(r)
+      ```
+   
+      Now, we have found the source of our time-consuming projection storage, with our graph containing 5,774,715 edges. This makes our graph projection very highly connected in general. Thinking back to what our projection represents, this makes sense – for each actor in our dataset, there are a number of HAS_COMMON_ACTORS relationships between films, based on every film they have appeared in. When an actor has appeared in, for example, five films, in our projection, this would generate a clique of five Film nodes. Considering that there are many actors that have appeared in far more films, the high connectedness of our shared actors projection is unsurprising.

+   3. We have a node attribute on our `Film` nodes in our Neo4j projection where a country is added to a list for each country of filming found in our knowledge graph data. We can use the `country` attribute to perform some analysis around common actors between films, and their filming locations.

+   4. Let’s write a Cypher query to find films with common actors that are filmed in separate countries, and find the most common pairs of locations. We can start with a MATCH statement, specifying