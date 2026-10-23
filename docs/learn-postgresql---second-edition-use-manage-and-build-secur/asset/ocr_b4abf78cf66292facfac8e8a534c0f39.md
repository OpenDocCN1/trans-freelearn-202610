# Using LEFT JOINS

We will now explore what a left join is. As an example, we can perform the following query:

```
forumdb=> select c.*,p.category,p.title from categories c left join posts p on c.pk=p.category;
-[ RECORD 1 ]----------------------------------------
pk          | 1
title       | Database
description | Database related discussions
category    | 1
title       | Indexing PostgreSQL
-[ RECORD 2 ]----------------------------------------
pk          | 1
title       | Database
description | Database related discussions
category    | 1
title       | Indexing Mysql
-[ RECORD 3 ]----------------------------------------
pk          | 3
title       | Programming Languages
description | All about programming languages
category    | 3
title       | Data types in C++
-[ RECORD 4 ]----------------------------------------
pk          | 2
title       | Unix
description | Unix and Linux discussions
category    | (NULL)
title       | (NULL)
-[ RECORD 5 ]----------------------------------------
pk          | 5
title       | Database
description | PostgreSQL
category    | (NULL)
title       | (NULL)
-[ RECORD 6 ]----------------------------------------
```