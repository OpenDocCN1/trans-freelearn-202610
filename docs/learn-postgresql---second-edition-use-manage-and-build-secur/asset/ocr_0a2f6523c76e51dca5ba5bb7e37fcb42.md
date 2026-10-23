This query, written using the NOT IN condition, looks for all records in the categories table for which the pk value does not match in the category field of the posts table. As we have already seen, another way to write the same query would be to use the NOT EXISTS condition:

```
forumdb=> select * from categories c where not exists (select 1 from posts where category=c.pk);
 pk | title       | description
----+-------------+-----------------------------
  2 | Unix        | Unix and Linux discussions
  4 | New Category | (NULL)
  5 | Database    | PostgreSQL
(3 rows)
```

If we now wanted to use a left join in order to achieve the same purpose, we would start by writing the following left join query:

```
forumdb=> \x
Expanded display is on.

forumdb=> select c.*,p.category from categories c left join posts p on p.category=c.pk;
-[ RECORD 1 ]---------------------------------
pk         | 1
title      | Database
description | Database related discussions
category   | 1
-[ RECORD 2 ]---------------------------------
pk         | 1
title      | Database
description | Database related discussions
category   | 1
-[ RECORD 3 ]---------------------------------
pk         | 3
title      | Programming Languages
description | All about programming languages
category   | 3
-[ RECORD 4 ]---------------------------------
pk         | 2
```