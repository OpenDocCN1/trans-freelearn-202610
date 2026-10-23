As mentioned earlier, it is preferable to use JOIN conditions whenever possible instead of IN or EXISTS conditions, because they perform better in terms of execution speed, as we will see in the following chapters.

# Using RIGHT JOIN

The right join is the twin of the left join, so we would have the same result if we wrote table A left join table B, or table B right join table A. For example, we can obtain the same results if we write:

```
select c.*,p.category from categories c left join posts p on p.category=c.pk;
```

or if we write:

```
select c.*,p.category,p.title from posts p right join categories c on c.pk=p.category;
```

as we can see here:

```
forumdb=> \x
Expanded display is on.

forumdb=> select c.*,p.category,p.title from posts p right join categories c on c.pk=p.category;
-[ RECORD 1 ]---------------------------------
pk          | 1
title       | Database
description | Database related discussions
category    | 1
title       | Indexing PostgreSQL
-[ RECORD 2 ]---------------------------------
pk          | 1
title       | Database
description | Database related discussions
category    | 1
title       | Indexing Mysql
-[ RECORD 3 ]---------------------------------
pk          | 3
title       | Programming Languages
description | All about programming languages
```