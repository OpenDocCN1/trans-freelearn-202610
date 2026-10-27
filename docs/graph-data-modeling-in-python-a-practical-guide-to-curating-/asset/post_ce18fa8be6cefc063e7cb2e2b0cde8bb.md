
![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_198_0.png)

In our original knowledge graph schema, before taking a projection, we wouldn’t be able to use any regular clique-finding algorithm for co-stars, as the nodes would have been related through two edges and a Film node. We could have written a custom, more complex method for finding co-star cliques using the original data structure, but this would likely have poorer performance too due to the additional edges it would have to traverse.

Instead, with our projection, we can use *igraph* and its `largest_cliques()` method to find the biggest fully connected subgraph inside our graph. This returns a list of nodes inside a set. So, let’s take the first element of that with a list slice to simplify the output.

In the following steps, we will explore the largest clique among actors:

1. We can take a look at the size of our biggest clique of co-stars and what nodes belong to it:

   ```python
   largest_clique = g.largest_cliques()[0]
   print(len(largest_clique))
   print(largest_clique)
   ```

2. Our print statements show that the largest clique in our projection is formed of 65 nodes. We can also see the node IDs belonging to that clique, but since it would be more meaningful to check what actors these nodes represent, let’s next use the `vs` attribute of our graph, g, and our `largest_clique` list to find the actor attribute for each of our clique nodes.

   This last step will reveal the actors who have all worked with each other in some capacity, in our dataset, during their careers:

   ```python
   clique_actors = g.vs[largest_clique]['actor']
   print(clique_actors)
   ```

We have carried out a lot of analysis on our *igraph* projection of co-starring actors and learned a lot about how actors work together in films by examining the structure of the graph.

In the next section, we have a second projection to analyze, which is complementary in some ways to our co-starring network.

The projection we previously set up in Neo4j represents films that share a common actor, so let’s dive into using our projection to gain insight into any interesting patterns present.

## Analyzing the Neo4j film projection

The projection we have in Neo4j contains `Film` nodes only, which share an edge if they have actors in common. Let’s begin, as we did in *igraph* for the co-stars projection, by characterizing some basic information about our graph.

The following steps will inform you of the Cypher queries you need to execute in the Neo4j Browser to get similar results to the Python analysis we have done in previous sections:

1. First, let’s count the number of nodes in the graph with a simple Cypher query in Neo4j Desktop, by using MATCH to find nodes of the `Film` type only, and returning the number of these nodes with the `count()` function:

   ```cypher
   MATCH (f:Film) RETURN count(f)
   ```

   The console output shows us that we have 79,478 Film nodes in our projection. When creating the projection, we had to write a batched query to read and write our data, and around 80,000 nodes wouldn’t usually cause us any issues.

2. Let’s also check how many HAS_COMMON_ACTORS relationships we created when building our projection, by matching this relationship between our `Film` nodes with a pattern. This time, we alias the relationship to r, and again return the number of nodes with `count()`:

   ```cypher
   MATCH (:Film)-[r:HAS_COMMON_ACTORS]->(:Film) RETURN count(r)
   ```

   Now, we have found the source of our time-consuming projection storage, with our graph containing 5,774,715 edges. This makes our graph projection very highly connected in general. Thinking back to what our projection represents, this makes sense – for each actor in our dataset, there are a number of HAS_COMMON_ACTORS relationships between films, based on every film they have appeared in. When an actor has appeared in, for example, five films, in our projection, this would generate a clique of five Film nodes. Considering that there are many actors that have appeared in far more films, the high connectedness of our shared actors projection is unsurprising.

3. We have a node attribute on our `Film` nodes in our Neo4j projection where a country is added to a list for each country of filming found in our knowledge graph data. We can use the `country` attribute to perform some analysis around common actors between films, and their filming locations.

4. Let’s write a Cypher query to find films with common actors that are filmed in separate countries, and find the most common pairs of locations. We can start with a MATCH statement, specifying only `Film` nodes and `HAS_COMMON_ACTORS` relationships that are part of our projection, and ensuring we are not querying the rest of the knowledge graph stored in the same database. Then, we can use WITH to access the country attributes of our Film nodes and alias them to c1 and c2. We are interested in films that share actors that have different filming locations, so we will use the <> operator to compare the countries. Then, using WITH to pair up the country lists, we can return country_pair alongside the count() of each unique pair, which implicitly aggregates our results. The very last step is to order the results by count(country_pair) in descending order so that we can focus on the most common pairings:

   ```cypher
   MATCH (f1:Film)-[:HAS_COMMON_ACTORS]->(f2:Film)
   WITH f1.country as c1, f2.country as c2
   WHERE c1 <> c2
   WITH [c1, c2] as country_pair
   RETURN country_pair, count(country_pair)
   ORDER BY count(country_pair) DESC
   ```

   In our results, we can see that films sharing actors often have very closely located countries of filming, with some top results being the United States and Canada. Language also seems to have an impact, with another frequent location pairing for our shared-actor films being the United States and the United Kingdom. Our most common result shows us a pair of `Cinema of Hong Kong` and `Hong Kong`, which hints to us that our data could have benefitted from additional cleaning – this is fairly common in scraped semantic data, and could help to direct data quality improvements in the original knowledge graph.

## Summary

In this chapter, we have used both Python and Neo4j to create, store, and analyze graph projections, demonstrating their power in both the efficiency and interpretability of results. Each technology has its own separate strengths. In Neo4j, we have less readily available access to complex graph data science algorithms to analyze our projection, compared to what we can easily carry out in Python with *igraph*.

However, using Neo4j is a more permanent storage option and suitable for a projection we might want to repeatedly read and write to. For any given use case, it is important to consider what the most appropriate projection creation and storage tool is for the task at hand.

These skills you have acquired will allow you to navigate between Neo4j, Python, and igraph with ease and will have set a strong foundation to build pipelines between the two technologies – a happy marriage indeed.

In the next, and final, chapter, you will learn about some of the common headaches of a graph data scientist. These lessons will focus on debugging (really, the only way to learn in data science is by creating imperfect code, and then going on a mission to debug this code, very much like Sherlock Holmes). Then, we will assist you by giving you insights into some of the common errors you will find when working in graph data science.

## Common Errors and Debugging

In this chapter, we will be working through common errors that you will be presented with as a newly qualified graph data scientist or engineer (which you will be when you have finished reading this book). The focus will be on how to debug graph issues. This will lead on to common issues and how to get around them when working in *igraph*. Following on from this, we will look at common Neo4j issues, as we have been working with igraph, Python, and Neo4j for a large proportion of this book; therefore, it is useful to know how to get around graph database issues as well. We will also touch on how to modify your *igraph* and Neo4j scripts to achieve the best performance.

There are many examples under each of these sections, with use cases of how and why these issues may present themselves. Therefore, you will be perfectly prepared, after reading this chapter, to debug and solve these common issues in your projects. In the upcoming sections, you will delve into common errors and issues you may face when working with igraph, Neo4j, and Python libraries for graph data modeling.

To summarize, in this chapter, we will be covering the following topics:

- Debugging graph issues
- Common igraph issues
- Common Neo4j issues

### Technical requirements

We will be using Jupyter notebooks to run our coding exercises, which require `python>=3.8.0`, along with the following packages, which will need to be installed in your environment with the `pip install` command:

- `neo4j==5.5.0`
- `igraph==0.9.8`
