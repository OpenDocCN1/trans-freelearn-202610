such as **Cypher**, **GraphQL**, or **Gremlin**. In this chapter, we will be using some basic Cypher to read and write data to a graph database. Neo4j has an extensive list of Cypher queries and their uses; see https://neo4j.com/developer/cypher/.

It is worth noting that while these query languages are powerful in terms of accessing and writing data, they do not currently offer the breadth of analyses and algorithms that a package such as *igraph* offers. In this chapter, we will be interfacing Python with a graph database, to demonstrate how *igraph* might be used in a complex analysis pipeline.

# Neo4j as a graph database

The graph database we will be using here is Neo4j, a commonly used high-performance database with a free local version. Neo4j can be queried using Cypher, which, of the graph query languages, is arguably the easiest for new users and so perfect for this introduction. For a more in-depth understanding of the usage of Cypher, see the documentation: https://neo4j.com/developer/cypher/.

Neo4j has analogous concepts to all of the graph concepts we have come across in previous chapters. In the Cypher documentation, while nodes are still named nodes, edges are more often referred to as relationships, and node and edge attributes are known as properties.

We have encountered several graphs with multiple types of nodes in heterogeneous graphs, and in Neo4j, these types of nodes have distinct *labels*. Labels are used heavily in the Cypher query language to access particular nodes and patterns; we will return to this concept when reading and writing to the graph database.

To get up and running with working with Neo4j, we need to carry out the following steps:

+   1. Neo4j can be installed using the instructions in the *Technical requirements* section of this chapter. This is shown in the following screenshot:

![](bbox=[0.11, 0.57, 0.83, 0.79])

Figure 5.1 – Neo4j database setup