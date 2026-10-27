![](bbox=[0.12, 0.09, 0.84, 0.33])

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