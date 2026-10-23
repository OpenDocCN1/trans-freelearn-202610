If the extension has been relocated, it’s necessary to specify the qualified name for every object, therefore prefixing the object name with the current extension schema. For example, after having relocated the extension into the my_schema namespace, the above command becomes:

```
forumdb=# ALTER EXTENSION pg_stat_statements
    DROP VIEW my_schema.pg_stat_statements;
ALTER EXTENSION
```

The result of the above DROP VIEW command is that the view initially created at extension deployment time is still there in the database and can be queried regularly; but this view is now an object with an independent lifecycle from the extension itself.

It is easy to see how the view is still available by simply querying it:

```
forumdb=# SELECT count(*) FROM pg_stat_statements;
count
------
    2
(1 row)
```

Of course, it is possible to add a new object to an extension with the ADD clause, which works as the opposite of the DROP one and requires the type and name of the object. For instance, to add the pg_stat_statements view back to the extension, it is possible to do the following:

```
forumdb=# ALTER EXTENSION pg_stat_statements
    ADD VIEW pg_stat_statements;
ALTER EXTENSION
```

You can also add your own objects to the extension. So, for example, adding a new table to the extension means that the extension will undergo the extension life cycle:

```
forumdb=# CREATE TABLE t_ext( i int, t text );
forumdb=# ALTER EXTENSION pg_stat_statements
    ADD TABLE t_ext;
ALTER EXTENSION
```

The t_ext table is now part of the extension, and as such, it cannot be manipulated anymore with statements that do not take the extension into account.