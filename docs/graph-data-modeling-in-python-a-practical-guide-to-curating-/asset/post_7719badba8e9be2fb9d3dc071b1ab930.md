
With our `print()` statement, we can take a look at some of the paths that have been found. Our `paths` variable contains the igraph IDs of nodes that are traversed from our original user node, as a list of lists.

> **Note**
> Some lists are not of length 4, which is what we are looking for to make recommendations. There is no sense in making recommendations of games our user has already purchased, as in paths of length 2, and no reason to recommend a user to our specified user, as in paths of length 3.

- 2. Because of this, we need to remove paths of length less than 4 from our `paths` list. We also only need the fourth element in each of these lists, which corresponds to a game we can base a recommendation on. We can achieve both of these requirements using a list comprehension with a conditional operator to ensure all path lengths are 4:

```
rec_game_ids = [path[3] for path in paths if len(path) == 4]
```

- 3. Now, we have a list of igraph IDs for nodes representing our games. We need the names of these games, so we will use another list comprehension to access the `steam_id` attribute of each node with `g.vs`:

```
game_names = [g.vs[game_id]['steam_id'] for game_id in rec_game_ids]
```

- 4. The preceding code will give us a list of game names. However, this list contains games that our specified user has already played or purchased, so we don’t want to recommend them. We can remove these games by first finding the neighboring nodes of our chosen user with the `neighbors()` method in igraph. We can then find the corresponding game names with a similar list comprehension as we used in the previous code snippet, looking for `steam_id` values with `g.vs`:

```
neighbors = g.neighbors(user_id_ex)
purchased_games = [g.vs[node_id]['steam_id'] for node_id in g.neighbors(user_id_ex)]
```

- 5. Let’s remove the games our user already plays with another list comprehension, comparing the lists:

```
game_names = [game for game in game_names if game not in purchased_games]
```

- 6. As in the previous example of using MySQL to accomplish the same thing, let’s take a look at the highest frequency games on the end of our paths by using Python’s `collections` library and `Counter()`:

```
from collections import Counter
game_frequency = Counter(game_names)
print(game_frequency)
```

Looking at the start of the printed `Counter()` object, we can see the top three games we might want to recommend to our user are Counter-Strike: Global Offensive, Unturned, and Portal 2.

In this section, we used a graph to perform a path-based analysis of our data. Where paths are involved, graphs are ideal data models to represent and query relationships between entities. We looked at simple ways to recommend games to users with our Steam data graph, but more sophisticated methods are typically used by recommendation engines in the real world.

Next, we will look at how our graph data model can help us implement a more savvy method for recommendations.

## Our recommendation system

Now that we have our data in a Python graph, let’s go one step further and design a more robust recommendation process, typically carried out with graph data.

Let’s take on the role of a solutions engineer or data scientist and write a game recommendation system based on our Steam data. Recommendation systems are used heavily in customer-facing applications, to show the user a product that they may be interested in. Product recommendations are often based on what behaviorally similar users have played and purchased.

## Generic MySQL to igraph methods

This time, we will write a set of reusable, generic methods to create an igraph graph from columns in a MySQL table. The functions will be designed to create a heterogeneous, bipartite, directed graph, given a set of column names.

Let’s start by writing a main function, `mysql_to_graph()`. The method will need to accept a MySQL table name, the table, source, and target column names of that table, and a column of edge weights. In this case, we will use the `steam_play` table, where our weights are the number of hours each game (target) has been played by a user (source). We will sequence these methods in a stepwise way to make them easier to implement:

- 1. In our new method, we can generate a SQL query with our desired search criteria by using an f string and replacement. The names of the columns after `SELECT`, and the table to select `FROM`, can now be dynamically set using the parameters accepted by `mysql_to_graph`. Then, we can use the method we defined in step 4 in the *Querying SQL with MySQL* section, called `query_mysql()`, to get a list of lists from our MySQL database:

```
def mysql_to_graph(table, source, target, weights, password):
    import igraph as ig

    sql_query = f'SELECT {source}, {target}, {weights} FROM {table}'

    data = query_mysql(sql_query, password=PASSWORD)
```

- 2. Now that we have data, we can use a series of list comprehensions to split the data into source nodes and target nodes. This helps us get the data in a sensible format for assigning igraph IDs. We will write a separate method, `create_igraph_ids()`, to create dictionaries of {Steam ID: igraph ID} pairs, after writing the main `mysql_to_graph()` method:

```
    source_nodes = sorted(list(set([source for source, _, _ in data])))
    target_nodes = sorted(list(set([target for _, target, _ in data])))

    source_igraph_ids = create_igraph_ids(source_nodes)
    target_igraph_ids = create_igraph_ids(target_nodes, len(source_igraph_ids))
```

- 3. Then, we need to generate an igraph edge list using the pairs in the created dictionaries, `source_igraph_ids` and `target_igraph_ids`. We can do this using another list comprehension, using sources and targets as keys. Lastly, before creating a graph, we must extract the edge weights from the list of lists returned by MySQL, ready to add to the graph edges later:

```
    edges = [(source_igraph_ids[source], target_igraph_ids[target])
        for source, target, _ in data]
    weights = [weight for _, _, weight in data]
```

- 4. Now, all the data is in the correct format to be added to an igraph graph. First, we must create an empty, directed `Graph()` object using `ig.Graph()`. Then, we must add nodes with `g.add_vertices()` equal to the length of both of our node lists. 'Internal IDs' – in our case, Steam IDs – are then added to the nodes by accessing the `g.vs` attribute of our graph, using the `source_igraph_ids` and `target_igraph_ids` dictionary keys. Node types are added, again using `g.vs`, by creating lists of our type strings, source and target, which in terms of the Steam data correspond to user and game, respectively. Finally, edges can be added from our `edges` list, containing the source and target igraph IDs, and the edge weights and hours played can be added listwise by accessing the `g.es` attribute of the graph. Our graph, `g`, is then returned so that we can analyze it in a separate function:

```
    g = ig.Graph(directed=True)
    g.add_vertices(len(source_nodes + target_nodes))
    g.vs['internal_id'] = list(source_igraph_ids.keys()) + list(target_igraph_ids.keys())
    g.vs['type'] = ['source' for _ in source_nodes] + ['target' for _ in target_nodes]
    g.add_edges(edges)
    g.es['weight'] = weights

    return g
```

- 5. In the previous `mysql_to_graph` method that we defined, we made a call to a separate user-defined function, `create_igraph_ids()`, which we will need to define separately. We can create igraph IDs for each node similarly to how we did earlier in the chapter – that is, by using a dictionary comprehension and making use of Python’s `enumerate()` method. Then, all our `create_igraph_ids()` method needs as parameters is a list of node names and the index to `enumerate()` from:

```
def create_igraph_ids(nodes, from_index=0):
    igraph_ids = {internal_id: igraph_id for igraph_id, internal_id
                  in enumerate(nodes, from_index)}
    return igraph_ids
```

We now have a reusable, generic method to create a graph from MySQL data describing any relationships where there is a source, target, and weighting for that relationship. Going back to our Steam use case, in our graph, we have users, the games they have played, and the length of time they have spent playing them. This is enough information for us to make suggestions about what games existing users might like to purchase next.

Earlier in this chapter, we made a very simple recommendation for a new game, based on whether users have played the same games as our specified user and what games they have played. However, this is a very simple example of a recommendation system. Using our graph, we can implement a more robust system that can be productionized to take a user ID as input and recommend games with a more sophisticated method, namely Jaccard similarity. Other similarity methods can also be applied using igraph, such as Dice similarity and inverse log-weighted similarity, which are also effective methods for comparing groups of nodes. Jaccard similarity was chosen for its simplicity and general good performance, but feel free to experiment with different similarity methods.
