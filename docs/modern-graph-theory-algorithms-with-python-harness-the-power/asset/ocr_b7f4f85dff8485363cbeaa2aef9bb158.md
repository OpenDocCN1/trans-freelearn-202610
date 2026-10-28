Let’s consider a route from store 2 to store 5. Several possible routes exist (store 2 to store 3 to store 4 to store 5, store 2 to store 1 to store 4 to store 5, and store 2 to store 4 to store 5). The shortest possible route may not have the fewest stops along the way if all distances are relatively short. In this case, we can simply look at our network and see that store 2 to store 4 to store 5 is the shortest route. However, in very large networks, this would take a lot of time and likely involve much longer paths.

We can use the A* algorithm in NetworkX to find our shortest route by adding to Script 4.2:

```
#find shortest path from Store 2 to Store 5
nx.astar_path(STRS2,"store2","store5", weight='weight')
```

As expected, the A* algorithm finds the shortest path from store 2 to store 4 to store 5. Again, with a much larger network, we’d need an algorithm such as the A* algorithm to find the shortest route quickly. Even a network of 20 stores with several connections among stores would be problematic to compute by hand from a visualization of the network. We’ve seen how algorithms can help us find the shortest paths between two specific destinations. Let’s now take a look at the shortest paths that include stops at several different vertices. This type of solution is critical to many optimal routing problems and builds on our problem of finding the shortest paths between two different destinations.

# Traveling salesman problem

A natural extension of shortest paths is the shortest possible route that stops at each location. For instance, consider a produce truck that needs to stock all five of our stores. The shortest route that will stop at each of our five stores saves time and fuel for the driver and allows produce to arrive at each store in the shortest time frame.

The traveling salesman problem seeks to find the shortest route that stops at each location or the shortest route that stops at an arbitrary number of possible locations. In graph theory, this problem (and Euler’s problem) is related to cycles of a graph, which define a non-empty path that starts and ends at the same vertex.

In practice, algorithms are needed to find the shortest path, and for large problems, computational time and convergence to a solution can restrict the usage of most algorithms. NetworkX provides the Christofides algorithm as a solver, which finds the shortest spanning tree (network structured like a tree with no cycles) and then matches the vertices of the tree to find minimum distances.

Let’s find a solution to the traveling salesman problem for our original five-store network with Script 4.3:

```
#define traveling salesman algorithm and apply it to the stores's
#networks
tsp = nx.approximation.traveling_salesman_problem
tsp(STRS)
```