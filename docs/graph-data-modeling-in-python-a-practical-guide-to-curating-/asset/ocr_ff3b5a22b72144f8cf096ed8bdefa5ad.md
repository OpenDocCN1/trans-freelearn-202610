2. We first match the customer represented by c, by looking for their customer ID. In the same statement, we find the products our customer in question has already purchased by matching a PURCHASED edge to a Product node. The alias p is assigned to these products, and we use this node to find these products brands through HAS_BRANDS edges. These brands are in turn aliased to b, and our last pattern matching step is to use MATCH to traverse back onto Products associated with these brands, r. At this point, we need to make sure that we don’t recommend the same product as the user has already purchased, which will have the same brand. To accomplish this, we add a WHERE clause, specifying that we only want results where the Customer node does not already have a relationship to the recommended Product nodes. To finish the query, we RETURN the productID node attribute of nodes aliased to r, renaming it to productID. We also use DISTINCT to produce a set of results, ensuring no duplicate products are recommended. In the same method, we run the query and get the returned data by calling data() and assigning it to result, before using a list comprehension to access each product ID in the returned list of dictionaries, assigning them to a list, and returning it:

```
def rec_by_brand(c, connection):
    query = f'MATCH (c:Customer {{customerID: toInteger({c.customerID})}})' \
        '-[ :PURCHASED ]->(p:Product) ' \
        'MATCH (p)-[:HAS_BRAND]->(b:Brand) ' \
        'MATCH (b)<-[:HAS_BRAND]-(r:Product) ' \
        'WHERE NOT (c)-[:PURCHASED]->(r) ' \
        'RETURN DISTINCT r.productID as productID'

    result = connection.query(query).data()
    result = [product['productID'] for product in result]

    return result
```

3. Now we can test out the method, and get some results for a single customer. Once again, we open a connection with Neo4j Connect, and choose a single customer in our list to pass as a parameter to rec_by_brand():

```
connection = Neo4jConnect('bolt://localhost:7687', 'admin', 'testpython')
brand_recommendations = rec_by_brand(customers[10], connection)
connection.close()
print(brand_recommendations)
```

4. This should print a list of product IDs to recommend to the specific customer. At this point, we could validate this further by opening the Neo4j Browser and running a query to find paths between this customer and products with the same brand as previously purchased products, using similar logic to the query in our rec_by_brand() method:

```
MATCH (c:Customer {customerID:10010})-[:PURCHASED]->(p:Product)
MATCH (p)-[:HAS_BRAND]->(b:Brand)
```