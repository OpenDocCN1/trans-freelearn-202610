The following method uses subqueries:

```
forumdb=> select pk,title,content,author,category from posts where category in (select pk from categories where title ='Database');
-[ RECORD 1 ]-----------------
pk      | 1
title   | Indexing PostgreSQL
content | Btree in PostgreSQL is....
author  | 1
category | 1
-[ RECORD 2 ]-----------------
pk      | 2
title   | Indexing Mysql
content | Btree in Mysql is....
author  | 1
category | 1
```

The subquery is represented by the following:

```
forumdb=> \x
Expanded display is off.

forumdb=> select pk from categories where title ='Database';
pk
----
  1
  5
(2 rows)
```

This statement extracts the values pk=1 and pk=5 from the category table and the external query searches the records in the posts table that have pk=1 or pk=5. Similarly, if you wanted to search for all post values that do not belong to the Database category, you would have to perform the following statement:

```
forumdb=> \x
Expanded display is on.

forumdb=> select pk,title,content,author,category from posts where category not in (select pk from categories where title ='Database');
```