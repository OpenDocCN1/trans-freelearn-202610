If we use a filter on the **Industry** field, which is not indexed, and run the query plan again, we see that even though a full table scan is still required, the query is very efficient:

![](bbox=[0.112, 0.15, 0.85, 0.33])

Figure 6.23 – Cardinality shows 4, meaning four records will be returned that have Industry as apparel

Notice how using a filter improved performance. The notes about IsDeleted and Industry simply mean that these fields cannot be used for optimization because they are not indexed

Indexes will behave similarly and result in even more performant queries. In this example, we are using the Id field as an index:

![](bbox=[0.112, 0.5, 0.85, 0.69])

Figure 6.24 – Using an index in the query results in a very efficient query