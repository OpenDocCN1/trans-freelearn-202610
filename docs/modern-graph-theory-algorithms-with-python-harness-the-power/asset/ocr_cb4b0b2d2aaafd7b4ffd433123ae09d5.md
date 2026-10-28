![](bbox=[0.225, 0.108, 0.773, 0.374])

*Figure 8.4* shows an example of a dynamic lion network with an evolving structure of edges and vertices that can be stamped with time information. Python provides a handy package to create a network such as this. The `NDLib` package utilizes `D yne t x` to model time-evolving graphs, which utilizes several key primitives from `DyNetx` to create and analyze dynamic networks. This provides a powerful framework for working with dynamic networks in the real world. This is particularly valuable in fields such as epidemiology, social network analysis, and transportation systems, where understanding temporal dynamics is essential. Let’s explore this package by creating *Figure 8.4* using *Script 8.1*:

```
```python
#create Figure 8.4
#import packages
#install dynetx if needed
#!pip install dynetx
import dynetx as dn
import networkx as nx
import matplotlib.pyplot as plt
#create an empty graph
g = dn.DynGraph()
#add vertices and edges with time information added

#adding the edge ("D","E") at t=0 that vanishes at time e=2
g.add_interaction(u="D", v="E", t=0, e=2)

# adding some edges at time t=1
g.add_interactions_from([("C", "F"), ("F", "E")], t=1)
```
```