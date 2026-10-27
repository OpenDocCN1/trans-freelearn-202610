![](bbox=[0.22, 0.11, 0.78, 0.48])

Figure 2.1 – Undirected heterogeneous graph

The schema represents the possibility of each type of node having an edge between them. Edges looping back to each type of node (known as self-edges) show that relationships can also exist between nodes of the same type. Finally, the data types for each attribute are listed, which are the same for each node: ID as a unique key integer, and name as a string.

Now, it is time to implement this model in Python. We will read our data from the supporting GitHub notebook (https://github.com/PacktPublishing/Graph-Data-Modeling-in-Python/blob/main/CH02/02.1_Implementing_model.ipynb).

# Implementing the model in Python

In the following step, we will load in the data that we are going to be working with.

To begin creating this graph in Python, we can import the nodes from musae_facebook_target.csv, using the standard Python csv library:

```
import csv
with open('./data/facebook_large/musae_facebook_target.csv', 'r',
encoding='utf-8') as csv_file:
    reader = csv.reader(csv_file)
```