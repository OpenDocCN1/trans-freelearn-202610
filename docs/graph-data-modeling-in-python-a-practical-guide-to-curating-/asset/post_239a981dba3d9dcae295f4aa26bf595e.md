
![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_165_0.png)

Figure 7.6 – Neo4j Browser constraint

2. Let’s test to see what happens now when we attempt to violate the constraint’s rule, in the Neo4j Browser window. First, we can create a node with userID set to 100:

```
CREATE (u:User {userID: '100'})
```

Then, try to run this same query again. If the constraint is set up properly, the browser window should return something such as “Node(2) already exists with label User and property userID = '100'.” This constraint prevents writing a new node with an existing user ID by throwing an error.

3. In contrast, let’s try to use MERGE to add the same node:

```
MERGE (u:User {userID: '100'})
```

When run in the Neo4j Browser, we should see that there are no changes to the database. The MERGE statement has matched the existing node with ID '100' and has subsequently written no new data to the database.

4. Before continuing, we should clear out the test data we added to the database in this section by running the following in the Neo4j Browser, as we have done in previous chapters:

```
MATCH (n) DETACH DELETE n
```

Constraints are a great way to absolutely ensure data conforms to schema requirements such as the uniqueness of properties. With our uniqueness constraint set up on users, and more tight control over our graph data model, let’s next look at adding some Twitter data to Neo4j.

2. With functions ready to connect to our Neo4j database, we can think about populating our graph with data, first from our edgelist. We will need to open edges1.csv using the inbuilt Python csv library, and convert each row into a list, resulting in a list of lists:

```
import csv
with open('./data/edges1.csv', 'r') as c:
    reader = csv.reader(c)
    edgelist = [edge for edge in reader]
print(edgelist[:5])
```

3. Our print statement shows that edges have been read into Python correctly. We now need a way to transfer these user IDs and their FOLLOWS relationships into Neo4j, so we will write a separate function for this. The functions will take n, the ID of a user who follows another user, with ID m. It will also take a Neo4j connection established using Neo4jConnect. We can then use these parameters to assemble a simple Cypher query, using MERGE so that existing nodes and edges are not replicated. Each User node is first merged into the graph, followed by the FOLLOWS edge between them. Quotes are used around the user ID properties to ensure they are added as a string, as per the schema we designed in the last section. The last step is to use our connection to send the write query to the database:

```
def add_edge_neo4j(n, m, connection):
    cypher = f'MERGE (u1:User {{userID: "{n}"}}) ' \
             f'MERGE (u2:User {{userID: "{m}"}}) ' \
             'MERGE (u1)-[:FOLLOWS]->(u2)'
    connection.query(cypher)
```

4. Let’s test that this function is working by adding a single edge to the database from Python. We can use list slices to get the source and target of the first edge in our imported CSV:

```
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
add_edge_neo4j(edgelist[0][0], edgelist[0][1], connection)
connection.close()
```

5. If this worked successfully, you should see that two User nodes and a FOLLOWS edge have been added to Neo4j, when using the Neo4j Browser. To confirm, we could run the following in the Neo4j Browser Terminal:

```
MATCH (u:User) RETURN u
```

node attribute, using data read in from a CSV. We will first import the circles1.csv data, similarly to how we imported our edgelist, using the csv library:

```
with open('./data/circle1.csv', 'r') as c:
    reader = csv.reader(c)
    circles_raw = [row for row in reader]
print(circles_raw)
```

Our print statement shows that we have a list of lists, with each list containing the circle ID in the first element and the users that belong to it following that. We will need to parse this format into something more easily handled in a Cypher query.

8. Let’s define a quick function to convert this data format into pairs of circle ID and user ID. We will call this get_user_circle_pairs(), which will take just the data we read in from circle1.csv. We define an empty list and for each circle and its list of users, take the circle ID from index 0, as well as a user ID, using a list comprehension. Because the first element in the list resulting from this comprehension will contain the circle ID twice, we take forward a slice from the second element onward. We then append each pair to the pairs list and return it:

```
def get_user_circle_pairs(circles):
    pairs = []
    for circle in circles:
        circle_pairs = [[user, circle[0]] for user in circle]
[1:]
        [pairs.append(pair) for pair in circle_pairs]
    return pairs
```

9. Once this function is defined, we can pass our circle data to this function, and then we can print the relevant output to the console:

```
pairs = get_user_circle_pairs(circles_raw)
print(pairs)
```

The result of print() will show circle ID/user ID pairs. With this more consumable data format, we can move on to adding the circle ID property to the Users in our graph.

10. Once this function is defined, we can pass our circle data to this function, and then we can print the relevant output to the console.

11. We will need to define another function for adding data to Neo4j. Let’s call it add_circles_neo4j(). This will receive a user ID, a circle ID, and an authenticated Neo4j connection. As we did with add_edge_neo4j(), we will create a Cypher query using f strings to replace parts of a string. Our query will use MATCH to find a User node with a specific user ID, then use SET to add the circle property to that node. Then, all that remains is to execute the write query against the Neo4j database, using connection.query():

```
def add_circles_neo4j(user_id, circle_id, connection):
    cypher = f'MATCH (u:User {{userID: "{user_id}"}})' \
```

```
f'SET u.circle = "{circle_id}"'
connection.query(cypher)
```

+   12. With our function to add the `circle` property defined, let’s run `add_circles_neo4j()` for each user ID/circle ID pair:

```
for user_id, circle_id in pairs:
    connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
    add_circles_neo4j(user_id, circle_id, connection)
    connection.close()
```

+   13. Head over to the Neo4j Browser and confirm that the desired properties have been added for User, by implementing a Cypher query similar to the following:

```
MATCH (u:User) RETURN u.circle
```

Our graph database, as it’s currently set up, now contains our schema for Twitter data, before the hypothetical change to the Circles feature. Now, imagine a new feature is requested, to allow users to be members of more than one Circle. Changes will need to be made to our schema and database to reflect the data resulting from this new feature.

In the next section, we will cover how to make changes to the schema to allow the pre-schema changes to be updated effectively.

## Updating the schema

To best illustrate why we might favor one schema design over another, when it comes to allowing multiple Twitter Circles per user, we will imagine what queries we would need to run in order to retrieve this data. These will be outlined in these steps:

+   1. Currently, finding users in a specific circle, when a user can only have one, can be done with a simple MATCH, specifying the `circle` property:

```
MATCH (u:User {circle: '5'}) RETURN u
```

+   2. However, if we started to add to the `circle` property in order to store additional information on additional circles a user is part of, for example, as a list, we would need to change this query to include a WHERE statement and locate a specific circle:

```
MATCH (u:User)
WHERE '5' in u.circle RETURN u
```

Upon running the Cypher, the query in the Neo4j Browser window will look as follows:

![](img/bc74e41f90aaa6ff0a01f7249c87c1d5_171_0.png)

Figure 7.8 – Follower connections with the updated schema

3. Now, while this kind of query is acceptable when the number of circles per user is small, as it might be when this feature update is released, this could scale very poorly. As users become members of more and more circles, the database engine will need to search lists of increasing length in order to find members of a particular circle. Queries could also become very complicated if we were looking for users who share several circles, or perhaps if we were to try to find users that don’t follow each other but share a circle, in order to recommend following a new Twitter user. By planning ahead, we can avoid these performance pitfalls later on, as the database becomes increasingly populated over time.

4. Instead, we can refactor our database and change the schema to that in Figure 7.5, where circles are added as a node rather than a property. With this schema, we can take better advantage of our data, as path-based queries to find relationships between distant users and circles are more easily computed.

5. Importantly, considering in this example that our database is serving an active platform, we can’t do what we have done in previous chapters and run something like MATCH (n) DETACH DELETE n. We want to preserve the data currently in our database in order to continue supporting our application’s current state.

6. So, we will begin by adding data according to our new schema, and finish up when our hypothetical feature has been deployed, by removing data conforming to the old one-user-per-circle data model.
