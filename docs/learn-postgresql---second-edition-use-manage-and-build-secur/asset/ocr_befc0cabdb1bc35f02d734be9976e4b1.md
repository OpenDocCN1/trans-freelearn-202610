If you want to understand how to configure a new logical replication, you should use the first Docker environment; however, if you want to skip all the topics about configuration, you can use the second one. In the second Docker environment, which you can find on the publication server, you will find the forumdb database that we’ve used so far in the book; you’ll also find a logical replication of just the users table. Finally, the chapter18_physical_logical Docker environment will be used for the last section of this chapter.

# Understanding the basic concepts of logical replication

Logical replication is a method that we can use to replicate data based on the concept of identity replication. REPLICA IDENTITY is a parameter present in table management commands (such as CREATE TABLE and ALTER TABLE); this parameter is used by PostgreSQL to obtain additional information within WAL segments, to recognize which tuples have been eliminated and which tuples have been updated. The REPLICA IDENTITY parameter can take four values:

+   - DEFAULT
+   - USING INDEX index_name
+   - FULL
+   - NOTHING

The concept behind logical replication is to pass the logic of the commands executed on the primary machine to the server and not the exact copy of the blocks to be replicated, byte by byte. At the heart of logical replication, there is a reverse engineering process that, starting from the WAL segments and using a logical decoding process, is able to extrapolate the original SQL commands and pass them on to the replication machine, using a logical decoding process.