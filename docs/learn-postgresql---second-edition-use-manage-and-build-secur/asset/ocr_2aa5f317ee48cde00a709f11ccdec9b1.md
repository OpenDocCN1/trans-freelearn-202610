```
forumdb=> create temp table t_posts as select * from posts;
SELECT 3
```

+   4. As we have done before, let’s create a new table named inserted_post with the same data structure as the posts table:

```
forumdb=> create table inserted_posts as select * from posts limit 0;
SELECT 0
```

+   5. Suppose now that we want to perform a SQL query that moves, in the same transaction, all the records that are present in the t_posts table to the inserted_posts table. This query will be as follows:

```
forumdb=> with ins_posts as ( insert into inserted_posts select * from t_posts returning pk) delete from t_posts where pk in (select pk from ins_posts);
DELETE 3
```

As we can see from the results, the query has achieved our goal:

```
forumdb=> select pk,title,category from t_posts ;
pk | title | category
-----+--------+----------
(0 rows)

forumdb=> select pk,title,category from inserted_posts ;
pk | title | category
-----+--------------------------------+----------
1 | Indexing PostgreSQL | 1
2 | Indexing Mysql | 1
3 | A view of Data types in C++ | 3
(3 rows)
```

# Query recursion

In PostgreSQL, it is possible to create recursive queries. Recursive queries are used in graph databases and in many common use cases, such as querying tables that represent website menus. Recursive CTEs make it possible to have recursive queries in PostgreSQL.