This sounds fun, so let's get started and tame this graph problem like a snake charmer would.

# Examining the graph structure

Now that we have our data in a graph model, we can consider some network-based analysis. A good place to start is examining some general properties of the graph, to get an idea of its structure.

It is common practice in network analysis to take a look at the degree distribution of a graph. Each node has a degree centrality, which in its simplest form is the number of nodes that it shares an edge with. Hence, the degree distribution is essentially a histogram of node-degree centralities, for a whole graph.

This will be detailed in the steps hereunder and relates to the graph we created in the previous section. In the supporting Jupyter notebooks, these functions have been saved in graphtastic.utils and can be imported easily by using graphtastic.utils import graph_from_attributes_and_edgelist.

Handily, igraph has a built-in way to quickly construct and print a degree distribution for a given graph, using the degree_distribution method on a Graph() object:

```
histogram = g.degree_distribution(bin_width=5)
print(histogram)
```

The bin_width parameter can be changed to any integer and specifies how fine-grained the histogram plot is.

Our histogram plot shows us that in our mutual Facebook likes graph, there are 8,324 nodes that are connected to a maximum of 4 other nodes (note that the (0, 5] range notation shown on the left side of the printed histogram is exclusive of nodes with a degree centrality of 5). At the other end of the scale, our most connected node has a degree centrality of somewhere between 705 and 709, with the second most connected node having a degree centrality of between 675 and 679.

Let's plot these degree frequencies in a slightly more appealing format, using the matplotlib library:

```
import matplotlib.pyplot as plt
bins = 30
plt.hist(g.degree(), bins)
plt.xlabel('Node degree centrality')
plt.ylabel('Frequency')
plt.show()
```