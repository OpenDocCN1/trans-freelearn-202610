Running this query returns information about the actor, the movie, and the relationship we just created between the two datasets, as shown in Figure 12.16:

![](bbox=[0.115, 0.148, 0.85, 0.49])

Figure 12.16 – The result of Figure 12.15’s Cypher query to connect Brad Pitt to Seven Years in Tibet

The next page of the Neo4j Cypher guide shows an alternative way to create this relationship. You are encouraged to modify that code to add another actor and movie as an exercise. Let’s turn our attention to Cypher’s WHERE clauses, which function much as they do in SQL.

We’ll search for persons whose names start with the first name Brad, which should pull up Brad Pitt’s entry, as well as any other actors whose first name is Brad. Figure 12.17 shows this query:

![](bbox=[0.115, 0.66, 0.85, 0.75])

Figure 12.17 – An example of a WHERE query in Cypher