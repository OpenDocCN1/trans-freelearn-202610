As can be seen here, PostgreSQL is now able to create an index access using a like query. The same thing happens for all types of like and ilike queries; the pg_trgm extension solves the access index for tables of this type of query. For further information about the pg_trgm extension, see https://www.postgresql.org/docs/current/pgtrgm.html.

The pg_trgm extension facilitates the DBA’s work in all those cases where they need to optimize like and ilike queries. Now, we will move on to the next extension, postgres_fdw.

# Using foreign data wrappers and the postgres_fdw extension

Foreign data wrappers allow us to access data that is hosted on an external database as if it were kept in a normal local table. We can connect PostgreSQL to various data sources, we can connect PostgreSQL to another PostgreSQL server, or we can connect PostgreSQL to another data source that can be relational or non-relational. Once the foreign data wrapper is connected, PostgreSQL is able to read the remote table as if it were local. There are foreign data wrappers for well-known databases such as Oracle and MySQL, and there are foreign data wrappers for lesser-known systems. A complete list of foreign data wrappers available for PostgreSQL is available at https://wiki.postgresql.org/wiki/Foreign_data_wrappers.

In this section, we will consider an example using the postgresql_fdw foreign data wrapper, which is used to connect a PostgreSQL server to another PostgreSQL server.

If we want to use the Docker images, we have to open two Bash terminals, and on the first one, we have to execute:

```
chapter19$ bash run-pg-docker.sh chapter19-postgresql_fdw
postgres@pg_fdw1:~$ psql -U forum forumdb
forumdb=> select * from categories;
pk | title | description
----+-------+-------------
(0 rows)
```

Then, on the second Bash terminal, we have to execute:

```
chapter19$ bash run-pg-docker-pg_fdw2.sh chapter19-postgresql_fdw
postgres@pg_fdw2:~$ psql -U forum forumdb
```