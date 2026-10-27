2. Calling this method for one customer should return a list of product IDs they have purchased:

```
connection = Neo4jConnect('bolt://localhost:7687',
    'admin', 'testpython')
purchases = get_customer_purchases(
    customers[10].customerID, connection)
connection.close()
print(purchases)
```

3. To make sure this is correct, we could compare the printed list to that found by a simple, equivalent query in the Neo4j Browser:

```
MATCH (c:Customer {customerID:10010})-[:PURCHASED]-(p:Product)
RETURN DISTINCT p.productID as productID
```

4. Now that get_customer_purchases() is working, we can get all customer purchases by using a dictionary comprehension over all customers in our list of Customer objects. For every customer, we call our method, and pass our Neo4j connection:

```
connection = Neo4jConnect('bolt://localhost:7687',
    'admin', 'testpython')
all_purchases = {customer.customerID:
    get_customer_purchases(customer.customerID,
    connection)
    for customer in customers}

print(all_purchases)
```

This results in a dictionary with keys containing customer IDs, and values containing lists of product IDs that a customer has purchased.

5. We next need to define a function to calculate the Jaccard similarity between these sets of purchased products. We can do this according to the standard implementation, which requires two sets, so first, the parameters list1 and list2 are converted to Python sets. Next, we find the intersection between set1 and set2, as well as finding the union. Finally, the Jaccard similarity is calculated by dividing the length of the set's intersection by the length of the set's union, and we return the value:

```
def jaccard_similarity(list1, list2):
    set1 = set(list1)
    set2 = set(list2)
    intersection = set1.intersection(set2)
    union = set1.union(set2)
    jaccard = len(intersection) / len(union)
    return jaccard
```