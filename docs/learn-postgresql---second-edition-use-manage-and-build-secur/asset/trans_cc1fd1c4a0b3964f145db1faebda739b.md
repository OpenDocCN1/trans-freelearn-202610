
关于聚合的概念，在下一章中，我们将看到使用窗口函数创建聚合的新方法。通过使用窗口函数，我们将能够创建本章描述的所有聚合和聚合函数，但我们也会看到，我们还可以选择创建新的聚合和聚合函数。

## 验证你的知识

- 如果我们在名为 mytable 的表上运行此查询且数据未更改，我们是否总能得到相同的结果？

  `select * from mytable`

  不，不一定，因为数据的顺序可能不同。

  更多细节参见 *探索 SELECT 语句* 一节。
- 查询结果是否可能只有 3 条记录？

  是的，可以使用 LIMIT 子句实现。

  更多细节参见 *使用 limit 和 offset* 一节。
- 如果有两张表：A 表有 3 条记录，字段 id 为主键；B 表有 2 条记录，字段 id 为主键，为了匹配 A 表和 B 表中所有 ID 相同的记录，我们应该使用哪种连接？

  我们必须使用内连接查询：

  `select tableA.id from tableA inner join tableB using(id)`

  更多细节参见 *使用 INNER JOIN* 一节。
- 如果有两张表：A 表有 3 条记录，字段 id 为主键；B 表有 2 条记录，字段 id 为主键，使用 NOT EXISTS 子句，我们如何编写一个查询来显示 A 表中存在而 B 表中不存在的所有记录？

  `select * from tableA where not exists (select 1 from tableB where tableA.id=tableB.id)`

  更多细节参见 *子查询与 EXISTS/NOT EXISTS 条件* 一节。
- PostgreSQL 11 和 PostgreSQL 16 使用 CTE 的方式是否相同？

  不相同。PostgreSQL 11 总是物化数据。PostgreSQL 16 在未指定时，仅当 CTE 在查询中被调用两次或更多次时才物化数据。

```
select * from mytable
```

```
select tableA.id from tableA inner join tableB using(id)
```

```
select * from tableA where not exists (select 1 from tableB where tableA.id=tableB.id)
```

更多细节参见 *子查询与 EXISTS/NOT EXISTS 条件* 和 *探索 CTE* 部分。

## 参考资料

- 子查询表达式官方文档：https://www.postgresql.org/docs/current/functions-subquery.html
- 连接官方文档：https://www.postgresql.org/docs/current/tutorial-join.html
- CTE 官方文档：https://www.postgresql.org/docs/current/queries-with.html
- MERGE 官方文档：https://www.postgresql.org/docs/current/sql-merge.html
- MERGE ANSI 2003 SQL：https://www.w3resource.com/sql/sql-syntax.php

## 在 Discord 上了解更多

要加入本书的 Discord 社区——在这里你可以分享反馈、向作者提问并了解新版本——请扫描下面的二维码：

https://discord.gg/jYWCjF6Tku

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_185_0.png)

## 使用基础语句窗口函数

在上一章中，我们看到聚合函数的运行方式如下：

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_188_0.png)

图 6.1：标准分组聚合

数据首先被排序，然后进行聚合；数据随后通过聚合被扁平化。当我们以 forum 用户连接到 forumdb 数据库后执行以下语句时，发生的情况如下：

```
forumdb=> select category,count(*) from posts group by category order by category;
category | count
----------+-------
        1 |     2
        3 |     1
(2 rows)
```

或者，我们可以决定通过执行以下语句来使用窗口函数：

```
forumdb=> select category, count(*) over (partition by category) from posts order by category;
category | count
----------+-------
```

```
1 | 2
1 | 2
3 | 1

(3 rows)
```

窗口函数创建聚合时不会将数据扁平化到单一行。然而，它们会将聚合结果复制到分组函数所引用的所有行。PostgreSQL 的行为如下图所示：

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_189_0.png)

这就是为什么如果我们想获得与经典 GROUP BY 查询相同的结果，必须在前面的查询中添加 distinct 关键字。

## 使用 PARTITION BY 函数和 WINDOW 子句

现在让我们使用窗口函数运行一些基本查询。假设我们想使用两个 over 子句。例如，如果我们想在一列上统计与类别相关的行数，并在另一列上统计总计数，那么我们必须运行以下语句：

```
forumdb=> select category, count(*) over (partition by category),count(*) over () from posts order by category;
 category | count | count
----------+-------+-------
     1    |   2   |   3
```

```
1 | 2 | 3
3 | 1 | 3

(3 rows)
```

或者如果我们想去除所有重复行，则必须运行以下语句：

```
forumdb=> select distinct category, count(*) over (partition by category),count(*) over () from posts order by category;
  category | count | count
-----------+-------+-------
        1 |     2 |     3
        3 |     1 |     3

(2 rows)
```

在前面的查询中，第一个窗口函数使用 category 字段聚合数据，而第二个窗口函数聚合整个表的数据。

使用窗口函数，可以在同一个查询中对不同字段的数据进行聚合。

正如我们在这里看到的，我们可以直接在查询级别定义窗口框架，但也可以为窗口框架定义别名。例如，前面的查询变成下面的形式：

```
forumdb=> select distinct category, count(*) over w1 ,count(*) over w2 from posts WINDOW w1 as (partition by category),w2 as () order by category;
  category | count | count
-----------+-------+-------
        1 |     2 |     3
        3 |     1 |     3

(2 rows)
```

使用别名的做法称为 WINDOW 子句。当有很多聚合时，WINDOW 子句非常有用。

## 介绍一些有用的函数

窗口函数可以使用我们在上一章中探讨过的所有聚合函数。除此之外，窗口函数还引入了新的聚合函数。

在探讨其中一些新聚合函数之前，让我们先介绍一个独特的函数——generate_series。generate_series 只是生成一个数值序列，例如：

```
forumdb=> select * from generate_series(1,5);
 generate_series
-----------------
                1
                2
                3
                4
                5
(5 rows)
```

在以下示例中，我们将在各种用例中使用此函数。

## ROW_NUMBER 函数

现在让我们来看一下 ROW_NUMBER() 函数。ROW_NUMBER() 函数为每个分区内的每一行分配一个递增编号：

```
forumdb=> select category, row_number() over w from posts WINDOW w as (partition by category) order by category;
 category | row_number
----------+------------
        1 |          1
        1 |          2
        3 |          1
(3 rows)
```

在前面的查询中，我们使用了 PARTITION BY 子句，根据 category 列中的值将窗口划分为多个子集。可以看到，我们有两个类别值：1 和 3。这意味着我们有两个窗口，在每个窗口内，ROW_NUMBER() 函数会按照我们之前定义的方式分配编号。

## ORDER BY 子句

ORDER BY 子句对窗口内的值进行排序。我们还可以使用 NULLS FIRST 或 NULLS LAST 选项，让 null 值出现在排序结果的开头或末尾。例如，我们可以执行一个没有 ORDER BY 子句的窗口函数查询，如下面的代码片段所示，但我们必须注意正在使用的函数类型以及我们的目标是什么。

