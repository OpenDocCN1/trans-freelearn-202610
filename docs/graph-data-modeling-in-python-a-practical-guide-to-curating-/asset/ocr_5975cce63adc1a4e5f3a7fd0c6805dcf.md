Perhaps surprisingly, this assert statement will cause an error, which tells us that this node is out of range of the object. This will produce a similar error message as hereunder:

```
ValueError: vertex index out of range
```

4. Though we know that node 1075 is contained in the select() results, using select() reindexes nodes in the returned VertexSeq. Node 1075 has instead been reindexed into the 0th element, and can actually be accessed as such:

```
sample = degree_100.select([0])
print(list(sample))
```

We won’t necessarily know what reindexing has taken place when running select(), so rather than searching for or further sampling the resulting VertexSeq objects using node IDs, it is best practice to sample by a node property, such as a unique identifier added to each node in the original graph. Reindexing will have no effect on accessing a node by its attribute, and so chaining select() would be safer.

In the next section, we will look at issues arising from path lengths and work through a case where this may occur.

# Efficiency and path lengths

Not all graph algorithms are equal, with each allowing the calculation or estimation of different features of nodes, edges, or an entire network. Nor are they equal in complexity, where some can run in milliseconds, and some take hours to compute.

When choosing a measure to learn something about the nodes in a graph, we need to take into account the computational complexity of the method used to measure a feature. This is often highly correlated with the size and connectivity of the graph.

To illustrate this point, we can compare the degree centrality and betweenness centrality algorithms, which both seek to calculate the importance of nodes in a graph, but in different ways. Degree centrality is a simple measure, counting each node’s connected neighbors, and scales proportionally to the number of nodes in a graph.

On the other hand, betweenness centrality traverses paths between all pairs of nodes in the graph, and so scales not only with the number of nodes, but the number of edges. In addition, due to its routine of finding short paths between nodes to estimate node importance, it is far more computationally demanding than calculating degree centrality.

While degree centrality is good for finding important hub nodes with many incident edges, betweenness centrality can show us nodes that act as important thoroughfares for paths through a graph. These concepts are linked and often correlate, but are not identical. However, betweenness centrality will always take longer to compute on a graph than degree centrality, by several orders of magnitude. It is