We can also create or update a record in the database using the MERGE statement with a CREATE and a MATCH statement to either create a new record or update an existing record (depending on what exists in the database). We’ll add Brad Pitt’s information back into the database using the code shown in Figure 12.14:

![](bbox=[0.13, 0.185, 0.865, 0.29])

Figure 12.14 – An updated example in Cypher code

When we run the code in Figure 12.14, we see Figure 12.9 appear again, where Brad Pitt’s information has been added to the database. Now that we know some basic Cypher commands, we can move on to more complicated graph database operations in the next section.

# More complicated query examples

Now that we know how to execute basic queries, we can move on to more complicated operations in Neo4j, such as adding relationships between items. Let’s connect actors and movies with a Cypher query connecting Brad Pitt to Seven Years in Tibet through his character, Heinrich Harrer.

> Note
> You’ll need to add both the movie and actor back into our database using the code in the Basic query example section before running this query. This query is shown in Figure 12.15.

![](bbox=[0.13, 0.625, 0.865, 0.73])

Figure 12.15 – A Cypher query to connect Brad Pitt to his character, Heinrich Harrer, in Seven Years in Tibet