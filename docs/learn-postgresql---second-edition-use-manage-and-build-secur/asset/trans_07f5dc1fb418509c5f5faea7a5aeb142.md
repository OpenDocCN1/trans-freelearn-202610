
```
3 | Programming Languages | All about programming languages
(1 row)
```

上述查询返回所有 pk > 2 的记录。

我们可以在 SELECT 语句中使用的另一个条件是 like 条件。下面我们来看一下。

## 使用 like 子句

假设我们想查找所有 title 字段值以字符串 Prog 开头的记录。

为此，我们必须使用 like 条件：

```
forumdb=> \x
Expanded display is on.
forumdb=> select * from categories where title like 'Prog%';
-[ RECORD 1 ]---------------------------------
pk          | 3
title       | Programming Languages
description | All about programming languages
```

如上所示，上述查询返回所有 title 以字符串 Prog 开头的记录。类似地，如果我们想查找所有 title 以单词 Languages 结尾的记录，则需要编写以下语句：

```
forumdb=> select * from categories where title like '%Languages';
-[ RECORD 1 ]---------------------------------
pk          | 3
title       | Programming Languages
description | All about programming languages
```

这两种搜索也可以组合使用。例如，如果我们想搜索所有包含部分字符串 discuss 的记录，可以这样写：

```
forumdb=> \x
Expanded display is off
forumdb=> select * from categories where description like '%discuss%';
pk | title   | description
----+---------+----------------------------
```

```
1 | Database | Database related discussions
2 | Unix | Unix and Linux discussions
(2 rows)
```

这里给出的查询将返回所有 description 包含字符串 discuss 的记录。

现在让我们尝试运行以下查询，看看会发生什么：

```
forumdb=> select * from categories where title like 'prog%';
(0 rows)
```

如我们所见，搜索没有返回任何结果。这是因为 like 搜索是区分大小写的。

现在让我们介绍 upper(text) 函数。upper 函数接收一个输入字符串，并返回将所有字符转换为大写后的相同字符串，如下所示：

```
forumdb=> select upper('prog');
upper
------
PROG
(1 row)
```

> 在 PostgreSQL 中，可以不写 FROM 就调用函数。PostgreSQL 不需要虚拟表来执行 SELECT 函数。如果是在 Oracle 数据库中，同样的查询就必须写成这样：select upper('prog') from DUAL;

回到前面的示例，如果我们想执行不区分大小写的 like 搜索，就必须编写以下语句：

```
forumdb=> select * from categories where upper(description) like '%DISCUSS%';
pk | title | description
----+-------+-----------------------------
1 | Database | Database related discussions
2 | Unix | Unix and Linux discussions
(2 rows)
```

至此，我们已经介绍了可以使用 like 运算符执行的所有功能。

## 使用 ilike

在 PostgreSQL 中，可以使用 ilike 运算符执行不区分大小写的 like 查询。在这种情况下，我们的查询将变成这样：

```
forumdb=> select * from categories where description ilike '%DISCUSS%';
 pk | title | description
-----+-------+-----------------------------
 1 | Database | Database related discussions
 2 | Unix | Unix and Linux discussions
(2 rows)
```

这是 PostgreSQL 解决我们之前遇到的不区分大小写 like 查询问题的方法。

## 使用 distinct

我们现在将讨论另一种查询：distinct 查询。不过，首先我们需要介绍另一个对 DBA 非常有用的函数——coalesce 函数。coalesce 函数在给定两个或更多参数时，返回第一个不为 NULL 的值。

例如，让我们对测试值使用 coalesce 函数：

```
forumdb=> select coalesce(NULL,'test');
 coalesce
----------
 test
(1 row)
```

在上述查询中，coalesce 函数返回 test，因为第一个参数为 NULL，而第二个参数不为 NULL。

现在，让我们插入一个新类别：

```
forumdb=> insert into categories (title) values ('New Category');
INSERT 0 1
```

然后让我们执行以下查询：

```
forumdb=# \pset null (NULL)
Null display is "(NULL)".
```

```
forumdb=> select pk,title,description from categories;
 pk |      title      |        description
----+---------------+-----------------------------
  1 | Database      | Database related discussions
  2 | Unix          | Unix and Linux discussions
  3 | Programming Languages | All about programming languages
  4 | New Category  | (NULL)
(4 rows)
```

在上面的示例中，对于 title 为 New Category 的记录，description 字段的值为 NULL。

现在让我们尝试使用 coalesce 函数来显示值 No Description 而不是 NULL。

```
forumdb=> select pk,title,coalesce(description,'No description') from categories;
 pk |      title      |        coalesce
----+---------------+-----------------------------
  1 | Database      | Database related discussions
  2 | Unix          | Unix and Linux discussions
  3 | Programming Languages | All about programming languages
  4 | New Category  | No description
(4 rows)
```

在上述代码中，coalesce 函数将任何 NULL 值转换为字符串 No description。coalesce 函数另一个不太友好的地方是，调用函数时给出的字段名并不是我们在查询中想要的名称。在这种情况下，结果集的第二个字段名为 coalesce，这并不是我们想要的名称；因为如果我们在团队中工作，更希望使用人类可读的名称。

在 PostgreSQL 中，可以为查询中的任何字段指定别名。例如，我们可以如下为 coalesce 字段指定别名：

```
forumdb=> select pk,title,coalesce(description,'No description') as description from categories;
 pk |      title      |        description
----+---------------+-----------------------------
  1 | Database      | Database related discussions
  2 | Unix          | Unix and Linux discussions
  3 | Programming Languages | All about programming languages
```

```
4 | New Category | No description
(4 rows)
```

现在结果集中显示的是 description 字段，而不是 coalesce 字段。

如果我们想使用包含空格或大写字母的别名，就必须使用双引号（""）将别名括起来，如下例所示：

```
forumdb=> select pk,title,coalesce(description,'No description') as "Description" from categories;
    pk |      title      |      Description
----+---------------+-----------------------------
  1 | Database      | Database related discussions
  2 | Unix          | Unix and Linux discussions
  3 | Programming Languages | All about programming languages
  4 | New Category  | No description
(4 rows)
```

结果集并没有使用别名 Description（大写），而是使用了别名 description（小写），这看起来不太对。

现在让我们再插入一条这样的记录：

```
forumdb=> insert into categories (title,description) values ('Database','PostgreSQL');
INSERT 0 1
```

让我们执行以下查询：

```
forumdb=> select title from categories order by title;
    title
-----------------
Database
Database
New Category
Programming Languages
Unix
(5 rows)
```

如上查询所示，有两条记录的值为 Database；如果我们想显示所有不同的值，就必须使用 DISTINCT 子句：

```
forumdb=> select distinct title from categories order by title;
    title
```

```
----------------
Database
New Category
Programming Languages
Unix
(4 rows)
```

在上述查询中，我们使用了 select distinct 语句。select distinct 语句仅用于返回不同的（互不相同的）值。在内部，distinct 语句涉及对大表进行数据排序，这意味着如果查询使用了 distinct 语句，随着记录数量的增加，查询可能会变慢。

## 使用 limit 和 offset

limit 子句是 PostgreSQL 中限制查询返回行数的方式，而 offset 子句用于跳过查询返回的指定行数。

limit 和 offset 用于从查询生成的结果集中返回一部分数据；limit 子句用于限制输出中的记录数，而 offset 子句用于告诉 PostgreSQL 从结果集中的哪个位置开始返回数据。

它们可以单独使用，也可以一起使用。

现在让我们通过以下查询来测试 limit 和 offset：

```
forumdb=> select * from categories order by pk limit 1;
pk | title | description
----+--------+-----------------------------
1 | Database | Database related discussions
(1 row)
```

上述查询只返回我们插入的第一条记录；这是因为 pk 字段是整数类型，其默认值始终按 identity 生成。

如果我们想查看插入的前两条记录，需要执行以下查询：

```
forumdb=> select * from categories order by pk limit 2;
pk | title | description
----+--------+-----------------------------
1 | Database | Database related discussions
2 | Unix | Unix and Linux discussions
(2 rows)
```
