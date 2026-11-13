![](bbox=[0.263, 0.092, 0.735, 0.495])

# Storage representation of Property Graph

Every element of the RDD of vertices contains a `vertexId` and an associated property and every element of the RDD of edges contains the ID of source vertex, ID of the destination vertex, and the associated property. The blue and green blocks represent the partitions of RDDs.

The following are some properties of the graph abstraction:

+   - As Property Graphs are stored as RDDs, they possess all the properties of the RDD such as partitioned and distributed, immutable, fault tolerant, lazy evaluation, and so on
+   - As vertices and edges are stored as separate RDDs, it makes it feasible to transform either vertices or edges without transforming the other
+   - During transformation, unaffected structure, attributes, and indices parts of the graph can be reused while creating new graphs
+   - IDs of vertices are used to partition the RDD range among different