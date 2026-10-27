On the other hand, for games, we are given the game’s name as a string. We must also add an igraph ID to distinct game nodes in the same manner as for user nodes.

We have already considered the schema design for nodes, but we must also consider how we should represent relationships. For our purposes, we are interested in the interactions between users and games. We have information on games purchased by users, as well as the length of time a user has played a game for.

Because of this, it is sensible to consider these different interactions as different edge types. Purchasing a game is represented just by the existence of a row in the steam_purchase table, and essentially the relationship either exists or doesn’t. We have no additional data to represent this interaction, so for game purchases, there will be no edge properties.

However, for relationships representing play time, we have information on how long a user played a game, in hours. In this case, we can use an edge property to represent the number of hours a game has been played by a user.

Taking all this into account, we can set out a schema for our graph, as follows:

![](bbox=[0.17, 0.55, 0.85, 0.65])

Figure 3.2 – Directed heterogeneous graph schema

Logically, since users either purchase or play games, in the graph schema, both the PLAYED and PURCHASED relationships are directional, from the User node to the Game node. For the remainder of this chapter, we will use this schema, but theoretically, it would make no difference if the relationships flowed from Games to Users, and instead had the names PLAYED_BY and PURCHASED_BY. The only downstream difference with this design would be that the direction of edges in any interrogation of the graph would need to be reversed.

# Ingestion considerations

Now that we have decided on a schema for our graph, we can begin to move data from our MySQL relational database to an igraph graph. The subsequent steps detail how to achieve this:

+   1. The first thing we must do is extract the data from our MySQL database and move it over to Python. We can do this using the query_mysql() method we wrote previously in this chapter. Now that we know more about the data, and that we have designed a graph schema, we can extract only the columns we need to create our graph:

```
play_query = 'SELECT id, game_name, hours FROM steam_play'
play_data = query_mysql(play_query, password=PASSWORD)
```