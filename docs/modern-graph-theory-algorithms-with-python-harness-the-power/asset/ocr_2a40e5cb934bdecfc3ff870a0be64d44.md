Script 4.1 gives us a network of our five stores with a nice plot summary showing distances between stores, as shown in Figure 4.9:

![](bbox=[0.21, 0.145, 0.795, 0.475])

Figure 4.9 – A plot of our five stores and the distances between each of them

We can now create an adjacency matrix based on our network and find the shortest paths between locations by adding to Script 4.1. Let’s calculate the shortest distances between store 1 and each other store in our chain’s locations:

```
#create adjacency matrix
adj1 = nx.adjacency_matrix(STRS)
adj1.todense()

#find all shortest paths starting at Store 1
length1, path1 = nx.single_source_dijkstra(STRS,"store1")
length1
```

From this calculation, we derive both the shortest distance between store 1 and all other stores (length1) and the shortest routes in general (path1). We can see the shortest distances between store 1 and the other stores from length1. The shortest route to store 2 takes two miles, while the shortest route to store 4 or store 5 takes three miles. store 3 is 2.4 miles from store 1.