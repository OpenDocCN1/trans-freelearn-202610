
when it is instantiated. We will pass a unique customerID to each customer, and use Faker to generate a name and address for the Customer object when it is created:

```
```python
class Customer:
    def __init__(self, customerID):
        self.customerID = customerID
        self.name = fake.name()
        self.address = fake.address()
```
```

+   4. Let’s test our new class by generating a customer. For testing purposes, we can create a customer with an arbitrary ID, 9999, and print the attributes of our Customer object:

```
```python
new_customer = Customer(9999)
print(new_customer.customerID)
print(new_customer.name)
print(new_customer.address)
```
```

+   5. Each time the Customer class is called, the __init__() method will automatically generate our customers’ names and addresses. With our new class, we now have the ability to quickly generate customers to populate our graph database.

+   6. To pass this customer information to our graph database, and for the rest of our pipeline, we will need to access Neo4j from Python, as we did in Chapter 5, Working with Graph Databases. When instantiated, this class establishes a connection to Neo4j using the credentials passed to it. It can also establish a session with Neo4j and run a query with the query() method, as well as closing the connection with close(). This is the same Neo4jConnect class we used in the previous chapter, so for more information on connecting Python to Neo4j, please consult Chapter 5, Working with Graph Databases. Albeit we have also included a custom package entitled Graphtastic where you can import this class quickly using from graphtastic.database.neo4j import Neo4jConnect. For transparency, the full implementation of this connection method is contained again in the following block:

```
```python
from neo4j import GraphDatabase
class Neo4jConnect:
    def __init__(self, uri, user, password):
        self.driver = GraphDatabase.driver(uri,
                      auth=(user, password))
    def close(self):
        self.driver.close()
    def query(self, query):
        session = self.driver.session()
        result = session.run(query)
        return result
```
```

+   7. We are now ready to generate and add customers to Neo4j. This can be thought of as a customer signing up to our online retail app or website. We next need the ability to pass our generated customer data to the graph database. To do this, we can create a method, `add_customer()`. The method will take a `Customer` object, `c`, and an authenticated Neo4j connection, made with our `Neo4jConnect` class. With these parameters, we can create a Cypher query to add a customer, substituting node attributes for the attributes of our customer, using `c.customerID`, `c.name`, and `c.address`. This is all handled with Python’s f string functionality, wrapping around a simple MERGE function. The additional curly braces used are to escape f string behavior in places, as Cypher syntax for specifying node attributes uses the `{ }` characters too. Looking at our graph schema, we also specify that the `customerID` attribute of our new node should be an integer, using Cypher’s `toInteger()`. The assembled query can then be called using `connection.query()`:

```
```python
def add_customer(c, connection):
    query = f'MERGE (:Customer {{customerID: toInteger({c.customerID}), '\
        f'name: "{c.name}", '\
        f'address: "{c.address}"}})'
    connection.query(query)
```
```

+   8. Let’s give this new method a try, by creating another example customer. Using our `Customer` class, we can generate a new `Customer` object with the attributes we need. As with all Cypher queries to Neo4j from Python, we then establish a connection with `Neo4jConnect`, before calling our new `add_customer` method with our generated `Customer` and our Neo4j connection, which handles writing a node to our graph database. Finally, we close the session with `connection.close()`:

```
```python
test_customer = Customer(9999)
connection = Neo4jConnect('bolt://localhost:7687',
    'admin', 'testpython')
add_customer(test_customer, connection)
connection.close()
```
```

+   9. Back in the Neo4j Browser, we can check whether the node was added correctly in a number of ways, but for something more visual, let’s open up Neo4j Desktop for the Store DB database that we have written to. Using some very simple Cypher to match all nodes of type Customer, we can return our customers to the browser:

```
```cypher
MATCH (c:Customer) RETURN c
```
```

We should see that one node is returned. Clicking on that Customer node should show the node properties on the right of the Neo4j Browser window, confirming that our attributes have been set correctly. Up next, we can now add customers to our graph, but at present they remain disconnected from the rest of the nodes in our database. According to our schema, we need to create relationships to represent product purchases, but we still need a process for simulating these transactions.

10. Let’s next create a method for adding a customer purchase, in a similar way to how we defined add_customer(). We can name our method add_purchase, and this time, we will require more parameters in order to model a purchase correctly. The function will take a customer, c, a product ID, the current time, and a Neo4j connection. We will again handle the assembly of a Cypher query with f strings. When a customer chooses a product and purchases it, to model this in our graph, we need to MATCH on the customerID attribute of the correct Customer node and MATCH on the productID of the matching Product node. These IDs are appropriate to match on as we have defined them as unique in our schema – no two customers or two products should share the same ID. The last step in our query is to MERGE an edge between our aliased Customer and Product nodes, c and p. We use a PURCHASED relationship to describe this edge and stamp the time parameter on the datetime edge attribute. Again, the last step is to send the query to our Neo4j database with connection.query():

```
def add_purchase(c, productID, time, connection):
    query = f'MATCH (c:Customer {{customerID: toInteger({c.customerID})}})' \
    f'MATCH (p:Product {{productID: toInteger({productID})}})' \
    f'MERGE (c)-[:PURCHASED {{datetime:"{time}"}}]->(p)'
    connection.query(query)
```

11. Now we can test the behavior of add_purchase(). Let’s use our already created Customer object with ID 9999, which is also already present in the database, and simulate a purchase. We can find a product for the customer to purchase in the products.csv raw data, or in Neo4j – for this example, we will use the node with productID 1010.

12. First, we import datetime from datetime, in order to access the current time at the point at which the purchase is made. We then establish a connection to the database. We use our add_purchase() method, and pass it our existing test_customer, along with productID 1010, the current time with datetime.now(), and the Neo4j connection. The addition of the edge will be handled by our function, so we can finish up by closing the connection:

```
from datetime import datetime
connection = Neo4jConnect('bolt://localhost:7687',
    'admin', 'testpython')
add_purchase(test_customer, 1010, datetime.now(),
    connection)
connection.close()
```

13. As we did before, let’s go into the Neo4j Browser for the Store DB database and see if we can find our newly created edge. We can again run some Cypher to find edges between Customer and Product nodes that have the edge type PURCHASED, and RETURN the nodes we matched:

```
MATCH (c:Customer)-[:PURCHASED]->(p:Product) RETURN c, p
```

If the edge was added correctly, in the browser, we should see the two nodes connected by a PURCHASED relationship. As we did with the newly added Customer node, we can check that the datetime attribute was correctly added to our new edge by clicking on it and examining its properties on the right-hand side of the Browser window.

+   14. In our graph schema, we noted that customers may want to make the same purchase twice and that this behavior should be represented by multiple edges between the same pair of Customer and Product nodes. We should make sure that this behavior is possible with our Python pipeline methods. To do this, let’s run the same code again, where `test_customer` purchases the product with ID 1010 a second time:

```
```python
connection = Neo4jConnect('bolt://localhost:7687',
    'admin', 'testpython')
add_purchase(test_customer, 1010, datetime.now(),
    connection)
connection.close()
```
```

+   15. When our code has run, let’s check that a new, separate relationship between our Product and Customer nodes has been added by running another Cypher query in the Neo4j Browser. We can either use the previous query we ran to visually display two edges between our nodes, or run a query to print customer and product details to a table, along with the datetime of any relationships between the nodes:

```
```cypher
MATCH (c:Customer)-[purchase:PURCHASED]->(p:Product)
RETURN c.name, p.name, purchase.datetime
```
```

We should see that two edges between this pair of nodes have been successfully added. In `add_purchase()`, we use MERGE to add the edge, and we learned in *Chapter 5, Working with Graph Databases*, that Cypher’s MERGE function won’t create a new node or edge if one matching the pattern specified already exists. In our graph, we can allow multiple edges by specifying datetime in our MERGE pattern. Because no edge with the exact date and time in milliseconds already exists between our pair of nodes, the pattern is not matched, and a new edge is created.

+   16. Our tests have shown us that `add_customer()` and `add_purchase()` are working as intended, and successfully writing customer data to our Neo4j database. Let’s quickly clean up the test customers and purchases we added to our graph before going any further. To do this, we can MATCH customer nodes specifically, preventing any of our product data from being deleted. We can then DETACH any relationships from them, and DELETE them from the database. This can be run from the Neo4j Browser:

```
```cypher
MATCH (c:Customer) DETACH DELETE c
```
```

+   17. With our custom methods set up, we now have everything in place to simulate the basic behavior of a customer using our application. However, in real scenarios, there are often many customers interacting with online retailers in a short space of time. We can next use the Python functions
