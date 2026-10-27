18. As always, we should confirm that our data has been added correctly. Recall that earlier in this chapter, we examined the number of games purchased by a specific user, with ID 151603712, which was 40. We can check if this is the same in our newly created graph by first finding the .index attribute of the node with a steam_id of 151603712 by using g.vs.select(). Then, we can find all edges of the PURCHASED type that have a source of this user ID, using g.es.select(), with two parameters to narrow down our search:

```
user_id_ex = g.vs.select(steam_id='151603712')[0].index
purchased_ex = g.es.select(_source_eq=user_id_ex, edge_type='PURCHASED')
print(len(list(purchased_ex)))
```

This print statement confirms that the len() property of the edges with this user as a source is 40, as in the steam_purchase dataset.

Now that we’ve added all the nodes and edges to our igraph.Graph() object, we are ready to analyze its properties.

# Path-based analytics in igraph

Graph data models are specialized for path-based analysis. Unlike in MySQL, which we covered earlier in this chapter, we do not need to make a series of queries to a database or perform a series of self-joins to find paths of related objects when using a graph. To demonstrate this, let’s use the graph we have created, with our new schema, to again find games our specific user may enjoy.

Last time, we only used data from the steam_purchase table, but this time, our graph contains data from both the steam_purchase and steam_play tables, so we can use information from both to make our recommendations. We will make recommendations for the user with ID 151603712 again, but this time using a graph-based method.

Our user node igraph ID is stored in the user_id_ex variable, so we can use this going forward. Instead of repeated queries or joins, here, we will be finding paths using the get_all_simple_paths() method in igraph. The paths we are looking for are those in Figure 3.1, from a specified user to the games that similar users play. These paths contain three hops from our initial user to our destination, which are the game nodes for recommendation.

The following steps will show how we can achieve this:

+   1. Let’s find these paths using get_all_simple_paths(). We need to specify the node to start paths from, as well as a cutoff for how long these paths should be. Not including the initial starting node, these paths are of length 3. We also need to specify the direction of the edges we are willing to travel in our paths using the mode parameter. Because all the relationships in our graph go from a user to a game, and we want to traverse multiple relationships in different directions to get our paths in this case, we made mode equal to all:

```
paths = g.get_all_simple_paths(user_id_ex, cutoff=3, mode='all')
print(paths[:10])
```