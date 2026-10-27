In the next section, we will look at path-based methods for dealing with the tabular data we looked at in this section. This will allow us to get insights into what types of games users enjoy playing on Steam.

# Path-based analytics in tabular data

Now that we have an idea of what our tables contain, we can ask some questions about our data. For example, we can ask, “What other games may a user enjoy, based on their previous purchase habits?” This type of question forms the basis of a recommendation system, used across the sales and marketing industry to drive additional customer purchases.

To answer a question like this simply, we will need to find the games a user has purchased, then find other users that have purchased the same games. Then, we will look at commonalities between those users to recommend a new game to our original user.

This type of system is often driven by a graph data model, due to the path-based operations required. The process of finding all games associated with a user, then users associated with those games, and finally the games those users associate with, represents a type of path-based query that graph databases are more suitable for than relational databases. In a relational database, we would have to repeatedly query the same table and perform a series of self-joins to find a series of relationships through users and games. We briefly explored these concepts in Chapter 1, Introducing Graphs in the Real World, but let’s also use this as a more practical example to demonstrate the differences between database types.

The following diagram shows the relationship between a user’s preferences and the games they play. We can use our graph recommendation engine to look at the games our user plays, and then look at what other users who play those games also play. Therefore, based on this tree, we can get an understanding of what games our (top node in the network) user may like based on similar users to them and the games they (all the other users) play:

![](bbox=[0.22, 0.75, 0.85, 0.9])

Figure 3.1 – Information required to make a game recommendation to a user