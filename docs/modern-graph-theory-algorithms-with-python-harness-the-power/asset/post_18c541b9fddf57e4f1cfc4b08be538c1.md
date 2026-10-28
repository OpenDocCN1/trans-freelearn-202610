
![](img/71ff737206f7d80bb625b3c9aca9dc7f_237_0.png)

Figure 12.4 – Neo4j download site

After hitting the Download button in Figure 12.4, you'll be prompted to follow the installer instructions to complete the installation. When the installer finishes, you'll see an icon or will have launched Neo4j directly on your machine from the installation process. This should take you to a page that looks like this:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_237_1.png)

Figure 12.5 – The start page of Neo4j

As shown in Figure 12.6, once you have launched the Movie DBMS database by clicking Start, you’ll have a connection to this database. It may take a few minutes to launch this database. It is possible that you may not be familiar with Neo4j before reading this chapter, so we’ll click on the about-movies.neo4j-browser-guide file option and run through a few examples of queries to become familiar with Cypher’s syntax and results.

You’ll see a page like Figure 12.7 when you click on the about-movies.neo4j-browser-guide file option. The first page shows a basic query pulling data on an actor. Here, we’ll change the actor to Brad Pitt and query the graph for instances of Brad Pitt. When you have the code changed to what is shown in Figure 12.7, hit the arrow to run the code. Note that you’ll see the default tutorial below the code you write. We aren’t showing this in our Neo4j query images but it will appear on your screen:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_239_0.png)

Figure 12.7 – A basic Cypher query that searches for Brad Pitt

This database does not contain any information about Brad Pitt. Running the suggested query on Tom Hanks does produce information in JSON form, as this data does not contain graph elements. You should see something like Figure 12.8 when you query the database for Tom Hanks:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_239_1.png)

Figure 12.8 – The result of our query for Tom Hanks in Movie DBMS

Let’s go to the next page of the guide and add an actor to the database. We’ll add Brad Pitt, as our query came back empty initially. Figure 12.9 shows how to add an actor to the database along with their birth year.

![](img/71ff737206f7d80bb625b3c9aca9dc7f_239_2.png)

Figure 12.9 – Instructions to create an entry for Brad Pitt

We can also create or update a record in the database using the MERGE statement with a CREATE and a MATCH statement to either create a new record or update an existing record (depending on what exists in the database). We’ll add Brad Pitt’s information back into the database using the code shown in Figure 12.14:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_242_0.png)

Figure 12.14 – An updated example in Cypher code

When we run the code in Figure 12.14, we see Figure 12.9 appear again, where Brad Pitt’s information has been added to the database. Now that we know some basic Cypher commands, we can move on to more complicated graph database operations in the next section.

## More complicated query examples

Now that we know how to execute basic queries, we can move on to more complicated operations in Neo4j, such as adding relationships between items. Let’s connect actors and movies with a Cypher query connecting Brad Pitt to Seven Years in Tibet through his character, Heinrich Harrer.

> Note
> You’ll need to add both the movie and actor back into our database using the code in the Basic query example section before running this query. This query is shown in Figure 12.15.

![](img/71ff737206f7d80bb625b3c9aca9dc7f_242_1.png)

Figure 12.15 – A Cypher query to connect Brad Pitt to his character, Heinrich Harrer, in Seven Years in Tibet

Running this query returns information about the actor, the movie, and the relationship we just created between the two datasets, as shown in Figure 12.16:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_243_0.png)

Figure 12.16 – The result of Figure 12.15’s Cypher query to connect Brad Pitt to Seven Years in Tibet

The next page of the Neo4j Cypher guide shows an alternative way to create this relationship. You are encouraged to modify that code to add another actor and movie as an exercise. Let’s turn our attention to Cypher’s WHERE clauses, which function much as they do in SQL.

We’ll search for persons whose names start with the first name Brad, which should pull up Brad Pitt’s entry, as well as any other actors whose first name is Brad. Figure 12.17 shows this query:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_243_1.png)

Figure 12.17 – An example of a WHERE query in Cypher

When we run the Cypher code in Figure 12.17, we should see two instances of Brad Pitt showing up in our database, as shown in Figure 12.18:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_244_0.png)

Figure 12.18 – Results from Figure 12.17's WHERE Cypher query

We can also match based on relationships, such as finding movies associated with an actor. Let's try a Cypher query on relationships to find all of the movies associated with Brad Pitt (here, just Seven Years in Tibet, as we added this relationship). Figure 12.19 shows the query we'll use to query a relationship:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_244_1.png)

Figure 12.19 – A Cypher query on graph relationships between people and movies

When we run the Cypher query shown in Figure 12.19, we see the connections between our instances of Brad Pitt and Seven Years in Tibet shown in graph form (Figure 12.20):

![](img/71ff737206f7d80bb625b3c9aca9dc7f_245_0.png)

Figure 12.20 – The results of Figure 12.19's graph relationship query

Figure 12.20 shows the essential strengths of a graph database. We can see relationships that connect different items across tables (here, the Person table and the Movie table), as well as the nature of that relationship. It’s easy to see an actor connected to a movie, and should more movies exist, we’d have an easy visualization of our query results from this graph-based query in Neo4j.

While this movie database contains basic tables and relationships, we can imagine our examples in the What can you represent in a graph database? section containing much more complicated relationships between many more tables. Visualizing query results in graph form across tables and items in tables provides easy-to-understand summaries of the data and relationships of interest in a query. Herein lies the power of graph databases. Queries tend to be quick, even for queries with many subqueries or when searching very large tables and their relations, and visualizations of results allow users to explore relationships in depth across tables (or provide succinct visuals for a report).

If you wish to explore Neo4j further, Neo4j’s AuraDB allows users to connect to the database through Python by establishing an account and connecting to Neo4j with an API in Python using the neo4j package and GraphDatabase.driver() function to create the connection.

**Note**
For any large databases or databases you wish to permanently save, you’ll need a paid account.

Once the database is created, you can query the Neo4j database through Python’s `neo4j driver.execute_query()` statement and import the data as a network in igraph to analyze the results. This provides a powerful combination of graph storage and graph analysis for your analytics projects. If you are interested, you are encouraged to explore Neo4j further and create your own connected projects between igraph and Neo4j’s AuraDB accounts.

## Summary

In this chapter, we examined cases where graph databases are advantageous, familiarized ourselves with an open source graph database called Neo4j, and learned a bit about the query language of Neo4j, called Cypher. We created, deleted, and modified records in a Neo4j movie database. We explored the advantages of querying graph databases and the unique query result visualizations possible with graph databases. If you are interested, I encourage you to consult Cypher and Neo4j resources to dive deeper into what is possible with graph databases.

In the next chapter, we’ll be putting together all of the skills we’ve learned in the book so far to tackle a real-world problem of predicting Ebola outbreak severity over time and geography across regions of the Democratic Republic of Congo.

## References

Adler, M. W., and Johnson, A. M. (1988). *Contact tracing for HIV infection.* British Medical Journal (Clinical research ed.), 296(6634), 1420.

Angles, R., Arenas, M., Barceló, P., Hogan, A., Reutter, J., and Vrgoč, D. (2017). *Foundations of modern query languages for graph databases.* ACM Computing Surveys (CSUR), 50(5), 1–40.

Beas-Luna, R., Novak, M., Carr, M. H., Tinker, M. T., Black, A., Caselle, J. E., ... and Iles, A. (2014). *An online database for informing ecological network models:* `http://kelpforest.ucsc.edu`. PloS one, 9(10), e109356.

Hyman, J. M., Li, J., and Stanley, E. A. (2003). *Modeling the impact of random screening and contact tracing in reducing the spread of HIV.* Mathematical biosciences, 181(1), 17–54.

Mueller, W., Rudowicz-Nawrocka, J., Otrzasek, J., Idziaszek, P., and Weres, J. (2016). *Spatial data and graph databases for identifying relations among members of cattle herd.* International Multidisciplinary Scientific GeoConference: SGEM, 1, 835–841.
