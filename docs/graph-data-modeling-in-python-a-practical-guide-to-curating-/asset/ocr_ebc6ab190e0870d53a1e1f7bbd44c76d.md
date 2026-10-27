# Summary

In this chapter, we compared and contrasted traditional relational databases and graph databases to perform path-based analyses. Our path-based analysis of choice was recommending a game to a user on the Steam publishing platform and was performed in both MySQL and igraph.

MySQL, and other relational databases, can be used to find paths between tables of related entities, such as users and the games they play and purchase. However, this involves performing self-joins on the same table or repeatedly querying the same table. On the other hand, graph databases and data models are natively set up for path-based queries, so we used igraph to recommend a game to a user based on paths between users and games in our graph.

Then, we covered how to move data over from MySQL to Python igraph, both step-by-step and with a generic set of methods that can be used for any directed, heterogeneous graph.

Finally, we set up a more sophisticated system to make game recommendations to users by using a graph data model. We based our recommendations on games similar users have played, using Jaccard similarity to estimate similarity. Our method allowed us to tune the results based on the edge weights in the graph, which corresponded to the play time for each user-game pair. This similarity-based recommendation system could be used in experiments to figure out how many hours a user should have played a game before it is allowed to influence the recommendations for Steam users.

In the next chapter, we will further concrete what you have learned up to this point. The core of the next chapter will be about using the graph to build a knowledge graph, where we will be looking at medical abstracts and using these documents to link terms and documents.