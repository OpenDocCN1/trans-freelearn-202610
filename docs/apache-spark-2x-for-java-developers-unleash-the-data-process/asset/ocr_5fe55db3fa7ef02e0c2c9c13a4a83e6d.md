# PageRank

**PageRank** is one of the most popular algorithms in graph theory. It is used to rank the vertices based on their importance. The importance of a vertex is calculated by the number of edges directed to the vertex. For example, a user is highly ranked on Twitter based on their followers, that is, the number of directed edges to that user vertex.

The PageRank algorithm was developed by Google founders _Larry Page_ and _Sergey Brin_ to measure the importance of web pages. Thus, the best example of PageRank implementation is the Google Search Engine. Google ranks pages based on their importance. For example, if page X contains hyperlinks to page Y then page Y has importance to page X as its rank increases. Google uses this algorithm very efficiently to rank pages and outputs highly ranked pages first based on keywords in a user's search query.

Spark Graphx provides the following two ways to execute PageRank.