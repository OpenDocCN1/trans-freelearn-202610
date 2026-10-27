
``` 
'MERGE (u)-[:MEMBER_OF]->(c)'
connection.query(cypher)
```

12. In this step, we can use add_circles_neo4j_new_schema() to add circle nodes to the graph, by looping over the user/circle pairs in pairs2:

```
for user_id, circle_id in pairs2:
    connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
    add_circles_neo4j_new_schema(user_id, circle_id, connection)
    connection.close()
```

13. With our new Circle nodes added, let’s move over to the Neo4j Browser and create a Cypher query to extract the user and circle information. Our new schema allows us to use MATCH to find paths, rather than searching through large property lists with WHERE, which is a more suitable type of query for a graph. Now, we can use MATCH to find users that have the MEMBER_OF relationship to a specific circle:

```
MATCH (u:User)-[:MEMBER_OF]->(c:Circle {circleID: '5'})
RETURN u, c
```

This returns the MEMBER_OF relationship to the specific circle, as visualized in the results of the Neo4j Browser:

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_173_0.png)

Figure 7.9 – MEMBER_OF relationship

14. The last step we need to do, now that we have rolled out our new graph data model to reflect our simulated changes to Twitter Circles, is to remove the old data. Imagine now that with our feature fully online, we are no longer using the circle property on User nodes, but rather the Circle nodes instead, to present information to an application’s frontend. Because of

From this point, we looked at building our initial schema (labeled a pre-change schema), which involved the combination of Python and Neo4j scripts (written in the Cypher query language).

Finally, the last stage was to take into account all that we have learned and apply the changes to our updated schema design.

This chapter focused on building everything you would need to handle evolving and updating schemas, which is something that you should bear in mind at design time as much as possible.

In the next chapter, we will further build on all the knowledge we have acquired so far when we look at graph projections and how they can be useful for downstream analytical tasks. These projections can contain analytically relevant, potentially aggregated, topological and property information of a main graph. This stored view will contain analytically relevant, potentially aggregated, and topological property information of the graph.

## Part 4: Graphing Like a Pro

In this part, we explore what graph projections are and how they can be used. This will give you the rudimentary knowledge you need to extract projections to perform analysis. These projections can then be extracted for further statistical and analytical uses. You will learn all about how to do this in this chapter. In addition, you will also get an understanding of how to export your projections in igraph and Neo4j. Buckle up and let the analysis flow with the use case looking at analyzing movies with relationships such as *acted in* and *starred with*.

Finally, the section on common errors and debugging jumps into the types of errors we have encountered in our careers, and how you can easily get around and resolve these errors and bugs effectively. Let’s think of this as your reference whenever anything untoward occurs in your code.

This part has the following chapters:

- *Chapter 8, Perfect Projections*
- *Chapter 9, Common Errors and Debugging*

## Perfect Projections

In this chapter, we are going to focus on creating graph projections, which you will be able to perform sophisticated analysis on, and even use other algorithms, such as machine learning and statistical methods, to form insights. We will start by explaining what projections are, leading on to how to use projections in practice. We'll draw upon how you can create projections in igraph and Neo4j, using a combination of the Cypher query language and Python.

For our use case, we will focus on popular movies, with a focus on using graph data science to find what films actors have appeared in, co-starred in, and multiple other relationships we can define with flexible graph structures. By the end of this chapter, you will be able to create a projection and put it to work for your use case.

We will be covering the following main topics:

- What are projections?
- How to use a projection
- Putting the projection to work

### Technical requirements

We will be using Jupyter Notebook to run our coding exercises, which requires `python>=3.8.0`, along with the following packages, which will need to be installed with the `pip install` command into your environment:

- `neo4j==5.5.0`
- `igraph==0.9.8`
- `matplotlib==3.6.3`

For this chapter, you will also need Neo4j Desktop installed. Please see the *Technical requirements* section of *Chapter 5, Working with Graph Databases* if Neo4j is not yet installed, as it will be needed to follow along with the tutorials in this chapter.

All notebooks, with the coding exercises, are available at the following GitHub link: `https://github.com/PacktPublishing/Graph-Data-Modeling-in-Python/tree/main/CH08`.

### What are projections?

Data in graph data models usually comprises relationships between things, whether they be people, languages, transport hubs, or any of the other examples we’ve seen throughout the previous chapters. Plenty of data models have several types of nodes and relationships, making one node or edge not equal in meaning or value to another.

These types of graphs are known as heterogeneous graphs, and we have seen them throughout this book. One type of heterogeneous graph is a bipartite graph, where there are two types of nodes. In bipartite graphs, only different types of nodes can share edges. For example, a citation network might be represented as a bipartite graph, with authors connected to articles they have written.

Analysis of heterogeneous graphs with multiple node and edge types can be tricky. To illustrate this, first consider the authorship graph in *Figure 8.1*. We can answer some simple questions easily, using this graph’s native structure, such as finding the number of articles an author has written using node degree, or contrastingly, the number of authors for an article, with the same method.

However, to measure the tendency for groups of authors to work together, for example, using an algorithm such as community detection (as we used in *Chapter 4, Building a Knowledge Graph*), we would need to traverse paths through both articles and authors in order to find sensible communities. The results from running an algorithm such as community detection across a bipartite graph may not necessarily make sense when trying to examine the connectedness of groups of solely authors or solely articles:

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_180_0.png)

Figure 8.1 – A sample from a bipartite authorship graph

To answer these types of questions that primarily concern the authors, rather than the articles themselves, we can use a projection. A projection is a representation of a graph – often a simplified version of a complex network. Projections often *collapse* graph data into more simplified interactions between specific entities, meaning that information is lost during the conversion. Although information is lost, analysis becomes easier and more intuitive and allows more direct questions to be asked of the graph.

*Figure 8.2* contains a unipartite projection of the bipartite graph shown in *Figure 8.1*. The graph now contains only **Author** nodes, which share an edge if the original bipartite graph nodes shared relationships to the same co-authored **Article**. Through this projection, we have lost certain information, for example, the individual articles that each pair of authors collaborated on. However, we have gained the ability to run graph algorithms over a homogeneous graph, for example, what was mentioned previously, community detection. We can sensibly split our graph into communities of authors algorithmically when our graph exclusively contains Author nodes:

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_181_0.png)

Figure 8.2 – Corresponding unipartite projection of the authorship graph

Other types of heterogeneous networks can be more complicated, with numerous node and edge types, such as in a knowledge graph.

A knowledge graph can represent a wealth of information in a highly complex schema – for example, a graph containing detailed customer information, which could contain many different ways in which information relates to each other.

*Figure 8.3* shows a sample of an example customer graph database, with many types of nodes and relationships. In this case, unlike the bipartite graph, a simple graph measure, such as the degree of certain types of nodes, would give you little useful information. If we find the degree of **Customer**
