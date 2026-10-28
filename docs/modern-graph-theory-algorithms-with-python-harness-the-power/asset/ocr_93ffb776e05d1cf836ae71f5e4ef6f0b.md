As shown in Figure 12.6, once you have launched the Movie DBMS database by clicking Start, you’ll have a connection to this database. It may take a few minutes to launch this database. It is possible that you may not be familiar with Neo4j before reading this chapter, so we’ll click on the about-movies.neo4j-browser-guide file option and run through a few examples of queries to become familiar with Cypher’s syntax and results.

You’ll see a page like Figure 12.7 when you click on the about-movies.neo4j-browser-guide file option. The first page shows a basic query pulling data on an actor. Here, we’ll change the actor to Brad Pitt and query the graph for instances of Brad Pitt. When you have the code changed to what is shown in Figure 12.7, hit the arrow to run the code. Note that you’ll see the default tutorial below the code you write. We aren’t showing this in our Neo4j query images but it will appear on your screen:

![](bbox=[0.115, 0.327, 0.855, 0.416])

Figure 12.7 – A basic Cypher query that searches for Brad Pitt

This database does not contain any information about Brad Pitt. Running the suggested query on Tom Hanks does produce information in JSON form, as this data does not contain graph elements. You should see something like Figure 12.8 when you query the database for Tom Hanks:

![](bbox=[0.115, 0.542, 0.855, 0.658])

Figure 12.8 – The result of our query for Tom Hanks in Movie DBMS

Let’s go to the next page of the guide and add an actor to the database. We’ll add Brad Pitt, as our query came back empty initially. Figure 12.9 shows how to add an actor to the database along with their birth year.

![](bbox=[0.115, 0.777, 0.855, 0.855])

Figure 12.9 – Instructions to create an entry for Brad Pitt