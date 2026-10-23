```
Access privileges |
Size | 7685 kB
Tablespace | pg_default
Description |
```

In the Size field, you can now see the real size of the database at that moment.

# The SQL method

When using the method outlined above, you may find that you cannot connect to your database through the psql command. This happens when we only have web access to the database; for example, if we only have pgadmin4 server-side installation access. If this happens, the SQL method is an alternative approach that will allow you to find the same information. To use this method, complete the following steps:

+   1. Execute the following command:

```
forumdb=> select pg_database_size('forumdb');
-[ RECORD 1 ]---+----------------
pg_database_size | 7869231
```

The pg_database_size(name) function returns the disk space used by the database called forumdb. This means that the result is the number of bytes used by the database.

+   2. If you wanted a more readable result in “human” terms, you could use the pg_size_pretty function and write the following:

```
forumdb=> select pg_size_pretty(pg_database_size('forumdb'));
-[ RECORD 1 ]---+----------------
pg_size_pretty | 7685 kB
```

As you can see, both methods give the same result.

# Behind the scenes of database creation

We have just learned what commands are used to create a new database, but what happens behind the scenes when a database is created?

In this section, we will see the relationships that exist between what we perform at the SQL level and what happens physically in the filesystem; note that the oid numbers we see below are related to the Docker image created. The numerical values of your Docker image could be different.