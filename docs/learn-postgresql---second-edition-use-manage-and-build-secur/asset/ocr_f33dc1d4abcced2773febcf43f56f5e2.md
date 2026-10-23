Let’s see a flow chart that shows how PostgreSQL internally executes queries:

![](bbox=[0.36, 0.13, 0.66, 0.42])

Figure 18.1: An illustration of the backend process

As we can see, a query, before being executed, requires several internal steps; this is because the system tries to execute the query in the best possible way, according to the conditions prevailing at that moment in the database. Now, suppose we want to replicate the data logically; at this point, we have two possibilities in front of us:

+   - We can capture commands before they get to the parser and transfer these commands to a second machine.
+   - We can try, in some way, to get the queries that are already parsed.

The first method is implemented by systems designed prior to native logical replication, which was based on triggers; an example of the application of this method can be found on Slony (https://www.slony.info/).