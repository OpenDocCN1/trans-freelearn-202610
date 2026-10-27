![](bbox=[0.15, 0.11, 0.78, 0.25])

Figure 1.3 – Two friend nodes are linked together with a single edge

In the preceding model, node properties are used to describe the number of followers Mark and Jeremy have, as well as the locations listed in their Twitter bios. This kind of additional node information is particularly important for querying graph data when asking questions that involve filtering.

In our Twitter example, properties would need to be present in the graph if, for example, we wanted to know who followed users with above 1,000 followers. We will revisit answering graphical questions using nodes, edges, and properties in later chapters.

Depending on the dataset, there may be cases where different nodes have different sets of properties. In this case, it is common to have several distinct types of nodes in the same graph.

# Heterogeneous graphs

Node types can also be referred to as layers, or nodes with different labels, though for this book, they will be known simply as types.

The following diagram shows the nodes representing Jeremy and Mark as people, where each node type has different properties, and there are multiple relationship types. Due to this, we can term these multiple relationships as heterogenous:

![](bbox=[0.22, 0.62, 0.78, 0.88])

Figure 1.4 – Example of a heterogenous Twitter graph