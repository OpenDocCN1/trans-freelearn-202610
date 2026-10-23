

如果我们使用不依赖排序顺序的聚合函数，例如 COUNT 函数，我们可以避免对数据进行排序；否则，最好在分区内部对数据进行排序，以避免每次查询启动时结果不同的风险：

```
forumdb=> select category,row_number() over w,title
from posts WINDOW w as (partition by category order by title) order by
category;
 category | row_number |             title
----------+------------+------------------------------
        1 |          1 | Indexing Mysql
        1 |          2 | Indexing PostgreSQL
        3 |          1 | A view of Data types in C++
(3 rows)
```

正如我们所见，在分区内部，数据按 title 字段排序。

## FIRST_VALUE

FIRST_VALUE 函数返回分区内的第一个值，例如：

```
forumdb=> \x
Expanded display is on.

forumdb=> select category,row_number() over w,title,first_value(title)
over w
from posts WINDOW w as (partition by category order by category) order by
category;
-[ RECORD 1 ]-----------------------------
category    | 1
row_number  | 1
title       | Indexing PostgreSQL
first_value | Indexing PostgreSQL
-[ RECORD 2 ]-----------------------------
category    | 1
row_number  | 2
title       | Indexing Mysql
first_value | Indexing PostgreSQL
-[ RECORD 3 ]-----------------------------
category    | 3
row_number  | 1
title  | A view of  Data types in C++
first_value | A view of  Data types in C++
```

## LAST_VALUE

LAST_VALUE 函数返回分区内的最后一个值，例如：

```
forumdb=> select category,row_number() over w,title,last_value(title) over w
from posts WINDOW w as (partition by category order by category) order by category;
-[ RECORD 1 ]---------------------------------
category  | 1
row_number | 1
title     | Indexing PostgreSQL
last_value | Indexing Mysql
-[ RECORD 2 ]---------------------------------
category  | 1
row_number | 2
title     | Indexing Mysql
last_value | Indexing Mysql
-[ RECORD 3 ]---------------------------------
category  | 3
row_number | 1
title     | A view of  Data types in C++
last_value | A view of  Data types in C++
```

在使用 first_value() 或 last_value() 函数时，务必始终使用 Order by 子句，以避免出现错误结果，如前所述。

## RANK

RANK 函数对分区内当前行进行排名，并存在间隔。如果我们不指定 PARTITION BY 子句，函数就不知道如何关联当前元组，因此函数会关联到自身，如下所示：

```
forumdb=> select pk,title,author,rank() over () from posts ;
pk |       title       | author | rank
----+--------------------------------+--------+------
 5 | Indexing PostgreSQL |    1 |   1
 6 | Indexing Mysql      |    1 |   1
7 | A view of  Data types in C++ |     2 |     1
(3 rows)
```

如果我们添加 order by 子句，函数会按指定顺序进行排名，例如，id 为 1 的作者从第 1 条记录开始，id 为 2 的作者从第 3 条记录开始，如以下示例所示：

```
forumdb=> select pk,title,author,rank() over (order by author) from posts ;
pk |      title      | author | rank
----+---------------+--------+------
5  | Indexing PostgreSQL |    1 |   1
6  | Indexing Mysql      |    1 |   1
7  | A view of  Data types in C++ |    2 |   3
(3 rows)
```

如果我们添加 PARTITION BY 子句，其工作机制相同；唯一的区别是排名是在分区内计算，而不是像前一个示例那样在整个表上计算：

```
forumdb=> select pk,title,author,rank() over (partition by author order by author) from posts ;
pk |      title      | author | rank
----+---------------+--------+------
5  | Indexing PostgreSQL |    1 |   1
6  | Indexing Mysql      |    1 |   1
7  | A view of  Data types in C++ |    2 |   1
(3 rows)
```

## DENSE_RANK

DENSE_RANK 函数与 RANK 函数类似。区别在于 DENSE_RANK 函数对分区内当前行进行排名，且不留间隔：

```
forumdb=> select pk,title,author,dense_rank() over (order by author) from posts order by category;
pk |      title      | author | dense_rank
----+---------------+--------+------------
5  | Indexing PostgreSQL |    1 |          1
6  | Indexing Mysql      |    1 |          1
```

## LAG 和 LEAD 函数

在本节中，我们将展示 LAG 和 LEAD 函数的工作方式。首先，我们将设置环境并生成一个数字序列，如之前所做：

```
forumdb=>  select x from generate_series(1,5) as x;
 x
---
1
2
3
4
5
(5 rows)
```

这是本示例的起点。官方文档（https://www.postgresql.org/docs/current/functions-window.html）将 LAG 函数定义如下：

> LAG 函数返回在分区内当前行之前 offset 行处计算的值；如果不存在这样的行，则返回默认值（必须与该值类型相同）。offset 和默认值都相对于当前行进行计算。如果省略，offset 默认为 1，默认值为 null。

现在，让我们编写以下语句：

```
forumdb=> select x,lag(x) over w from (select generate_series(1,5) as x) V WINDOW w as (order by x) ;
 x | lag
----+-----
 1 |
 2 | 1
 3 | 2
 4 | 3
 5 | 4
(5 rows)
```

正如我们所见，lag 函数返回的结果集的偏移值为 1。如果我们引入偏移参数，lag 函数将返回偏移等于我们作为输入传递的数字的结果集，如下一个示例所示：

```
forumdb=> select x,lag(x,2) over w from (select generate_series(1,5) as x)
V WINDOW w as (order by x) ;
 x | lag
----+-----
 1 |
 2 |
 3 | 1
 4 | 2
 5 | 3
(5 rows)
```

lead 函数与 lag 函数相反，如官方文档所述：

> “LEAD 函数返回在分区内当前行之后 offset 行处计算的值；如果不存在这样的行，则返回默认值（必须与所提及的值类型相同）。offset 和默认值都相对于当前行进行计算。如果省略，offset 默认为 1，默认值变为 null。”

以下是几个示例，可以看到其工作原理。在第一个示例中，我们将使用 lead 函数而不带任何参数：

```
forumdb=# select x,lead(x) over w from (select generate_series(1,5) as x)
V WINDOW w as (order by x) ;
 x | lead
----+------
 1 | 2
 2 | 3
 3 | 4
 4 | 5
 5 |
(5 rows)
```

正如我们在 lead 函数中所见，偏移从底部开始。

传递给 NTILE 函数的参数决定了我们希望桶由多少条记录组成。

现在，让我们通过尝试将结果集拆分为两个桶来看看它是如何工作的：

```
forumdb=> select x,ntile(2) over w from (select generate_series(1,6) as x)
V WINDOW w as (order by x);
 x | ntile
----+-------
 1 |   1
 2 |   1
 3 |   1
 4 |   2
 5 |   2
 6 |   2
(6 rows)
```

如果我们想将结果集分成三个桶，我们将运行以下语句：

```
forumdb=> select x,ntile(3) over w from (select generate_series(1,6) as x)
V WINDOW w as (order by x);
 x | ntile
----+-------
 1 |   1
 2 |   1
 3 |   2
 4 |   2
 5 |   3
 6 |   3
(6 rows)
```

NTILE() 函数接受一个整数，并尝试将窗口划分为数量均衡的桶，指定每一行属于哪个桶。

在本节中，我们介绍了一些功能，允许您进行一些基本的数据挖掘。例如，lag 和 lead 可用于比较表的不同行，因此可以比较不同员工的薪水，或比较不同天的收入。

在下一节中，我们将更深入地探讨窗口函数的一些更高级的功能。

