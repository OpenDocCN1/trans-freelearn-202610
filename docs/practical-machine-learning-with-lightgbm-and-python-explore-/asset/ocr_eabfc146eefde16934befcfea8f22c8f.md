With PostgresML, the architecture may be simplified as follows:

![](bbox=[0.27, 0.125, 0.825, 0.335])

Figure 10.2 – Combining the ML services with data storage using PostgresML allows for a more straightforward system design

Although this example is an oversimplification, the point is that significant overhead is added to the overall process of leveraging ML models in a service-oriented architecture with separate model services.

Through PostgresML, we can eliminate the need for separate model storage and overheads of loading models and, importantly, combine data storage calls and predictions into a single call on the data storage layer with no network overhead in between. PostgresML’s benchmarks found that the simplified architecture improved performance by a factor of 40 within a cloud environment [1].

However, there are also downsides to this architecture. First, the database is now a single point of failure. If the database is unavailable, all models and predictive capabilities are also unavailable. Second, the architecture mixes the concerns of data storage and ML modeling and inference. Depending on the use case, training and deploying ML models has different server infrastructure needs compared to serving SQL queries and storing data. The mixture of concerns might force you to compromise on one or the other responsibilities or significantly increase database infrastructure costs to support all use cases.

In this section, we introduced PostgresML and explained, at a conceptual level, the advantages of combining our data store and ML service. Now, we’ll look at practically setting up and getting started with PostgresML, alongside some basic functionality.

# Getting started with PostgresML

PostgresML, of course, relies on PostgreSQL being installed. PostgresML requires PostgreSQL 11, with newer versions also supported. PostgresML also requires Python 3.7+ to be installed on your system. Both ARM and Intel/AMD architectures are supported.