Let’s now try to make a DML command on the pg_pub server. Some examples of DML commands follow:

+   - INSERT
+   - DELETE
+   - UPDATE

For example, say we tried to delete a record from the t1 table of the pg_pub server:

```
db_source=# delete from t1 where id=5;
DELETE 1
```

On the pg_pub server, we would have the following:

```
db_source=# select * from t1;
 id | name    | description
----+---------+-------------
  1 | Linux   |
  2 | FreeBSD |
  4 | Minix   |
  3 | Windows |
(4 rows)
```

On the pg_sub server, though, we would still have this:

```
db_destination=# select * from t1;
 id | name
----+---------
  1 | Linux
  2 | FreeBSD
  4 | Minix
  3 | Windows
  5 | Unix
(5 rows)
```

If we examine postgresql.log on the pg_sub server, we’ll see this:

```
learn_postgresql_sub_1 | 2023-06-16 15:30:57.973 UTC [364] ERROR:
logical replication target relation "public.t1" is missing replicated
column: "description"
```