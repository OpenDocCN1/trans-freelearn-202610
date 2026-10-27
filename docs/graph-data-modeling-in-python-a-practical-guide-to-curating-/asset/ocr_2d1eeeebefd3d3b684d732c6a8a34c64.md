However, not all social networks are the same, and in some online social media platforms, relationships between users of a social network may not be mutual.

For example, on Twitter, one user can follow another, but this doesn’t mean the inverse must be true. On Twitter, Jeremy may follow Mark, but Mark may not follow Jeremy.

# Directed graphs

Here, a directional edge is used to show that Jeremy follows Mark, while the absence of an edge in the reverse direction shows that Mark does not follow Jeremy in return:

![](bbox=[0.3, 0.36, 0.75, 0.42])

Figure 1.2 – Two friend nodes are linked together with a single edge

This type of graph is known as a directed graph. For reference, sometimes, undirected edges like those in Figure 1.1 are shown as bidirectional edges, pointing to both nodes. This bidirectional representation is equivalent to an undirected edge in most senses and represents a mutual relationship.

Importantly, when creating a data model with directional edges, naming relationships appropriately becomes important. In our Twitter example, if the edge representing the interaction between Mark and Jeremy is follows, then the edge goes from the Jeremy node to the Mark node.

On the other hand, if the edge represents a concept such as followed by, then this should be in the other direction – that is, from Mark to Jeremy. This has particularly strong implications for some more complex graph modeling and use cases, which we will cover in Chapter 2, Working with Graph Data Models.

# Node properties

While nodes and edges (directional or not) are the basic building blocks of a graph, they are often not sufficient to fully describe a dataset. Nodes can have data associated with them that may not be relational, so it would not be expressed as a relationship with another node.

In these cases, to represent data associated with nodes, we can use node properties (sometimes known as node attributes). Similarly, where an edge has additional information associated with it, in addition to representing a relationship, edge properties can be used to hold that data.

The following diagram shows a black line, indicating that Jeremy follows Mark but that Mark does not follow Jeremy – therefore, the black line indicates directionality: