4. So, we have successfully added a new table to the template1 database. Now let’s try to create a new database called dummydb and make a list of all the tables in the dummydb database:

```
template1=# create database dummydb;
CREATE DATABASE
template1=# \c dummydb
You are now connected to database "dummydb" as user "postgres".
```

The dummydb database contains the following tables:

```
dummydb=# \dt
    List of relations
 Schema |   Name    | Type  |  Owner  
--------+-----------+-------+---------
 public | dummytable | table | postgres
(1 row)
```

As expected, in the dummydb database, we can see the table created previously in the template1 database.

> It is important to remember that any changes made to the template1 database will be present in all databases created after this change.

Now we will delete the dummydb database and the dummy table in the template1 database.

# Dropping tables and databases

In the next section, you will learn how to delete tables and databases. The commands we are going to learn are the following:

+   - DROP TABLE: This is used to drop a table in the database.
+   - DROP DATABASE: This is used to drop a database in the cluster.

# Dropping tables

In PostgreSQL, the command needed to drop a table is simply DROP TABLE tablename. To do this, we have to connect to the database to which the table belongs, and then run the DROP TABLE tablename command.