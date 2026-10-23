Another thing we must always keep in mind is that not all tables grow at the same rate or to the same level; some tables are bigger than others and some indexes too are bigger than other indexes.

We also need to know that there is a part of our server’s RAM, shared among all the PostgreSQL processes, that is used to manage the data present in tables. This part of the server’s RAM is called `shared_buffers`.

The way PostgreSQL works is as follows:

+   1. Data is taken from hard disks.
+   2. Data is placed in shared buffers.
+   3. Data is processed in shared buffers.
+   4. Data is downloaded to disks.

Typically, in a dedicated server only for PostgreSQL, the size of `shared_buffers` is about one-third or one-quarter of the total server RAM. A useful link to set some PostgreSQL configuration parameters (including a recommended size for `shared_buffers`) is https://pg tune.leopard.in.ua.

When a table grows excessively compared to the `shared_buffers` size, there is a possibility that performance will decrease. In this case, partitioning data can help us. Partitioning data means splitting a very large table into smaller tables in a way that is transparent to the client program. The client program will think that the server still has only a single table, but having smaller tables also means having smaller indexes that have higher chances of staying in memory, which in turn increases data performance; moreover, having smaller tables means that the vacuum processes works on smaller tables, which minimizes the execution time of the vacuum processes. Finally, when running a vacuum full, the disk space used by the table is doubled, therefore having many small tables instead of one large one significantly reduces any impact from this issue. Data partitioning can be done in two ways:

+   - Using table inheritance (the only possible way for PostgreSQL < 10)
+   - Using declarative partitioning (the best way starting from version 10)

After figuring out when it is recommended to partition data, let’s see what types of table partitioning are possible. PostgreSQL supports three types of declarative partitioning:

+   - Range partitioning
+   - List partitioning
+   - Hash partitioning

We will now describe these three methods in detail.