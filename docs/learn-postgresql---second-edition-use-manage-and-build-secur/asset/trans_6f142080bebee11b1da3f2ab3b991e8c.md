

如果我们只想获取插入的第二条记录，则需要执行以下查询：

```
forumdb=> select * from categories order by pk offset 1 limit 1;
 pk | title |        description
-----+-------+----------------------------
  2 | Unix  | Unix and Linux discussions
(1 row)
```

当我们希望以分页方式返回数据时，offset 和 limit 非常有用。

limit 的另一个重要功能是，它可以基于现有表创建新表。例如，如果我们想从 categories 表创建一个名为 new_categories 的表，则需要执行以下语句：

```
forumdb=> create table new_categories as select * from categories limit 0;
SELECT 0
```

该语句只会将 categories 表的数据结构复制到 new_categories 表中。

SELECT 0 子句表示没有数据被复制到 new_categories 表中；只有数据结构被复制了，如下所示：

```
forumdb=> \d new_categories
               Table "forum.new_categories"
   Column    |  Type   | Collation | Nullable | Default
---------------+---------+-----------+----------+---------
 pk           | integer |           |          |
 title        | text    |           |          |
 description  | text    |           |          |
```

## 使用子查询

在本节中，我们将讨论子查询。子查询可以描述为嵌套查询——我们可以使用括号将一个查询嵌套在另一个查询中。与常规查询一样，子查询可以返回单个值或记录集。我们首先介绍使用 IN/NOT IN 操作符的子查询。

### 子查询与 IN/NOT IN 条件

我们先从 IN 操作符说起；我们可以在 where 子句中使用 IN 操作符，而不必使用多个 OR 条件。例如，如果你想搜索所有 pk=1 或 pk=2 的分类，则需要执行以下语句：

```
forumdb=> select * from categories where pk=1 or pk=2;
 pk | title | description
-----+-------+-----------------------------
 1 | Database | Database related discussions
 2 | Unix | Unix and Linux discussions
(2 rows)
```

另一种达到相同结果的方法是：

```
forumdb=> select * from categories where pk in (1,2);
 pk | title | description
-----+-------+-----------------------------
 1 | Database | Database related discussions
 2 | Unix | Unix and Linux discussions
(2 rows)
```

与 IN 操作符相似但功能相反的操作符是 NOT IN 操作符。例如，如果我们想搜索所有不包含 pk=1 或 pk=2 的分类，则需要执行以下语句：

```
forumdb=> select * from categories where pk not in (1,2);
 pk | title | description
----+--------------------------------+-----------------------------
 3 | Programming Languages | All about programming languages
 4 | New Category | (NULL)
 5 | Database | PostgreSQL
(3 rows)
```

现在，我们可以向 users 和 posts 表插入一些数据：

```
forumdb=> insert into users (username,email) values ('luca_ferrari','luca@pgtraining.com'),('enrico_pirozzi','enrico@pgtraining.com');
INSERT 0 2
```

```
forumdb=> insert into posts (title,content,author,category) values ('Indexing PostgreSQL','Btree in PostgreSQL is....',1,1);
INSERT 0 1
forumdb=> insert into posts (title,content,author,category) values ('Indexing Mysql','Btree in Mysql is....',1,1);
INSERT 0 1
forumdb=> insert into posts (title,content,author,category) values ('Data types in C++','Data type in C++ are ..',2,3);
INSERT 0 1
```

posts 表中现有的记录如下：

```
forumdb=> \x
Expanded display is on.
forumdb=> select pk,title,content,author,category from posts;
-[ RECORD 1 ]-----------------------
pk      | 1
title   | Indexing PostgreSQL
content | Btree in PostgreSQL is....
author  | 1
category | 1
-[ RECORD 2 ]-----------------------
pk      | 2
title   | Indexing Mysql
content | Btree in Mysql is....
author  | 1
category | 1
-[ RECORD 3 ]-----------------------
pk      | 3
title   | Data types in C++
content | Data type in C++ are ..
author  | 2
category | 3
```

假设我们现在想查找所有属于 Database 分类的帖子。为此，我们可以使用多种方法。

以下方法使用子查询：

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

子查询如下所示：

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

该语句从 categories 表中提取 pk=1 和 pk=5 的值，外部查询则在 posts 表中搜索包含 pk=1 或 pk=5 的记录。类似地，如果你想查找 posts 表中所有不属于 Database 分类的记录，则需要执行以下语句：

```
forumdb=> \x
Expanded display is on.

forumdb=> select pk,title,content,author,category from posts where category not in (select pk from categories where title ='Database');
```

```
- [ RECORD 1 ]-----------------
pk      | 3
title   | Data types in C++
content | Data type in C++ are ..
author  | 2
category | 3
```

### 子查询与 EXISTS/NOT EXISTS 条件

当我们想检查子查询是否有返回结果（TRUE）时，使用 EXISTS 语句；当我们想检查子查询是否无返回结果（FALSE）时，使用 NOT EXISTS 语句。例如，如果我们想使用 EXISTS/NOT EXISTS 条件编写与之前相同的条件查询，则需要执行以下操作：

```
forumdb=> select pk,title,content,author,category from posts where exists (select 1 from categories where title ='Database' and posts.category=pk);
- [ RECORD 1 ]-----------------
pk      | 1
title   | Indexing PostgreSQL
content | Btree in PostgreSQL is....
author  | 1
category | 1
- [ RECORD 2 ]-----------------
pk      | 2
title   | Indexing Mysql
content | Btree in Mysql is....
author  | 1
category | 1
```

上述查询返回的结果与使用 IN 条件编写的查询结果相同。

类似地，如果我们想使用 NOT EXISTS 条件查找所有不属于 Database 分类的帖子记录，则需要编写以下内容：

```
forumdb=> select pk,title,content,author,category from posts where not exists (select 1 from categories where title ='Database' and posts.category=pk);
- [ RECORD 1 ]-------------
pk      | 3
title   | Data types in C++
```

```
content | Data type in C++ are ..
author | 2
category | 3
```

使用 IN 条件和 EXISTS 条件编写的查询都称为**半连接查询**，我们将在下一节中介绍连接。

## 了解连接

让我们来了解一下什么是连接、连接有哪些类型以及它们的用途。我们可以将连接理解为两个或多个表中行的组合。

例如，以下查询返回 categories 表的行与 posts 表的行的所有组合：

```
forumdb=> select c.pk,c.title,p.pk,p.category,p.title from categories c,posts p;
pk | title | pk | category | title
----+---------------+----+----------+----------------
1 | Database | 1 | 1 | Indexing PostgreSQL
2 | Unix | 1 | 1 | Indexing PostgreSQL
3 | Programming Languages | 1 | 1 | Indexing PostgreSQL
4 | New Category | 1 | 1 | Indexing PostgreSQL
5 | Database | 1 | 1 | Indexing PostgreSQL
1 | Database | 2 | 1 | Indexing Mysql
2 | Unix | 2 | 1 | Indexing Mysql
3 | Programming Languages | 2 | 1 | Indexing Mysql
4 | New Category | 2 | 1 | Indexing Mysql
5 | Database | 2 | 1 | Indexing Mysql
1 | Database | 3 | 3 | Data types in C++
2 | Unix | 3 | 3 | Data types in C++
3 | Programming Languages | 3 | 3 | Data types in C++
4 | New Category | 3 | 3 | Data types in C++
5 | Database | 3 | 3 | Data types in C++
(15 rows)
```

