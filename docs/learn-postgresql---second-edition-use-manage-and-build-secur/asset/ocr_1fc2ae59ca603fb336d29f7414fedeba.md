```
```text
where username='enrico_pirozzi')
select pk,title from posts_author_1;
 pk |          title
----+----------------------------
  3 | A view of Data types in C++
(1 row)
```
```

If we don’t specify any option, the default is NOT MATERIALIZED, and this could be a problem if we are migrating a database from a minor version to PostgreSQL 12. This is because the behavior of the query planner could change, and the performance could change too.

> From version 12, we have to insert the **MATERIALIZED** option if we want to have our queries display the same performance behavior that we had with the previous versions.

# CTE – use cases

Let’s now present some examples of the use of CTEs:

+   1. Firstly, we will create two new tables:
+      - `t_posts`, with all the records present in the post table
+      - `delete_posts`, with the same data structure as the posts table

```
```sql
forumdb=> create temp table t_posts as select * from posts;
SELECT 3

forumdb=> create table delete_posts as select * from posts limit 0;
SELECT 0
```
```

The starting values for the t_posts and delete_posts tables are as follows:

```
```text
forumdb=> select pk,title,category from t_posts ;
 pk |          title           | category
----+--------------------------+----------
  1 | Indexing PostgreSQL      |        1
  2 | Indexing Mysql           |        1
  3 | A view of Data types in C++ |     3
(3 rows)
```
```