```
print(play_data[:10])

purchase_query = 'SELECT id, game_name FROM steam_purchase'
purchase_data = query_mysql(purchase_query,password=PASSWORD)
print(purchase_data[:10])
```

In play_data, we have information on users, the games they have played, and the time they have spent playing each game. In purchase_data, we only need users and the games they have purchased.

Next, as stated in the process of schema design, to use igraph, we will need to add an increasing integer igraph node ID to both the User and Game nodes, starting from 0.

2. The following lines of code will generate a set() of unique user IDs and users in both the play_data and purchase_data lists. Then, a combination of a dictionary comprehension and the enumerate() method will generate a dictionary with keys containing Steam user IDs, and values containing our igraph IDs:

```
users = set([row[0] for row in play_data] + [row[0] for row in purchase_data])
user_ids = {user_id: igraph_id for igraph_id, user_id in enumerate(users)}
print(len(user_ids))
```

Printing the length of our dictionary with len() gives us the number of unique users, 12393.

3. As with the users, we also need to generate igraph IDs for games. We can use a very similar process to the preceding one, but while making a small change to the enumerate() method parameters. We can use the second parameter in enumerate() to generate increasing integers, but rather than starting at 0, we can start at the highest igraph ID assigned to users using the user_ids dictionary's len():

```
games = set([row[1] for row in play_data] + [row[1] for row in purchase_data])
game_ids = {user_id: igraph_id for igraph_id, user_id in enumerate(games, len(user_ids))}
print(len(game_ids))
```

As with users, our print statement shows the number of unique games in our datasets, 5155. Using enumerate() with the second parameter should avoid any ID conflicts between users and games. Even though their node types are different, igraph still requires IDs for all nodes to be increasing integers from 0.

4. We can check that IDs have indeed been created as expected by examining the highest igraph IDs for users, and the lowest igraph IDs for games, using the sorted() method and console print() statements:

```
print(sorted(user_ids.values(), reverse=True)[:10])
print(sorted(game_ids.values(), reverse=False)[:10])
```