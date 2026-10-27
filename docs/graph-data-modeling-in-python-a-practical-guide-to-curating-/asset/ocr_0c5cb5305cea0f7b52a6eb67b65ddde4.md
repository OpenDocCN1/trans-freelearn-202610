3. With our session set up, we are now ready to query the database. For this example, we will add the query parameter to be used to create the graph relationship to our Neo4j session:

```
query = 'CREATE (:Person {name: "Jeremy"})-[:FOLLOWS]->(:Person {name:"Mark"})'
result = session.run(query)
driver.close()
```

These lines of code won’t return anything, since we have performed a write operation on the Neo4j database. However, we can confirm that it was successful by either opening the Neo4j Browser or using a read query from Python. For this first query, we will demonstrate both, beginning with the browser.

4. Go back to the Neo4j Browser for our Python DB database and run the following query from the command line at the top of the window:

```
MATCH (n) RETURN n
```

This query essentially matches every node in the database and returns it for viewing. The nodes and relationships we added in Python will be displayed:

![](bbox=[0.12, 0.44, 0.85, 0.8])

Figure 5.2 – Neo4j Browser result