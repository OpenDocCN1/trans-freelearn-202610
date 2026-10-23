```
Schema | Name | Type | Owner
--------+-----------+--------+--------
forum | categories | table | forum
forum | j_posts_tags | table | forum
forum | posts | table | forum
forum | tags | table | forum
forum | users | table | forum
(5 rows)
```

# Making a new database from a modified template

Now that we’ve learned how to list all tables in a database, let’s ensure that any changes made to the template1 database will be seen by all the databases that will be created later. We will perform these steps:

+   1. Connect to the template1 database.
+   2. Create a table called dummytable inside the template1 database.
+   3. Create a new database called dummydb.

Let’s start making the database using the following steps:

1. Connect to the template1 database:

```
postgres@learn_postgresql:/$ psql template1
template1=#
```

2. As superuser, create a table called dummytable. For now, we don’t need to worry about the exact syntax for creating tables; this will be explained in more detail later on:

```
template1=# create table dummytable (dummyfield integer not null primary key);
CREATE TABLE
```

3. Use the \dt command to show a list of tables that are present in the template1 database:

```
template1=# \dt
List of relations
Schema | Name | Type | Owner
--------+-----------+--------+---------
public | dummytable | table | postgres
(1 row)
```