

ON CONFLICT 条件中的字段必须具有唯一或排除约束。之前的语句只是替换了以下语句：

```
INSERT INTO j_posts_tags (post_pk ,tag_pk) values (2,1)
```

它被替换为以下语句：

```
UPDATE set tag_pk=tag_pk+1 where tag_pk=1 and post_pk=2
```

## 学习 INSERT 的 RETURNING 子句

在 PostgreSQL 中，我们可以在 insert 语句中添加 RETURNING 关键字。PostgreSQL 中的 RETURNING 关键字提供了一个机会，在 insert 或 update 执行后返回该 insert 或 update 语句中任何列的值。例如，如果我们想要返回刚刚插入的记录的所有字段，我们需要执行如下查询：

```
forumdb=> insert into j_posts_tags (tag_pk,post_pk) values(1,3) returning *;
 tag_pk | post_pk
---------+---------
       1 |       3
(1 row)

INSERT 0 1
```

* 表示我们想要返回刚刚插入的记录的所有字段；如果我们只想返回某些字段，就必须指定查询需要返回的字段：

```
forumdb=> insert into j_posts_tags (tag_pk,post_pk) values(1,2) returning tag_pk;
 tag_pk
--------
      1
(1 row)

INSERT 0 1
```

在章节末尾讨论 CTE 时，此功能将显示出特别有用的价值。

## 从查询中返回元组

在之前的章节中，我们已经查看了简单的 update 查询，例如以下：

```
forumdb=> update posts set title = 'A view of  Data types in C++' where pk = 3;
UPDATE 1
```

现在我们将查看更复杂的内容。如果我们想要更新 posts 表中以某种方式相关的记录怎么办？

### 与多个记录相关的 UPDATE

让我们从以下场景开始：

- 1. 考虑 categories 表：

```
forumdb=> SELECT * FROM categories;
pk |       title       |        description
----+------------+----------------------------
 1 | Database          | Database related discussions
 2 | Unix              | Unix and Linux discussions
 3 | Programming Languages | All about programming languages
 4 | New Category      |
 5 | Database          | PostgreSQL
(5 rows)
```

- 2. 让我们考虑一个 categories 表，我们想要用它来更新现有的 categories 表的新表。

```
forumdb=> create temp table t_categories as select * from categories limit 0;
SELECT 0

forumdb=> insert into t_categories (pk,title,description) values (4,'Machine Learning','Machine Learning discussions'),(5,'Software engineering','Software engineering discussions');
INSERT 0 2
forumdb=> select * from t_categories ;
pk |       title       |        description
----+------------+----------------------------
```

```
4 | Machine Learning      | Machine Learning discussions
5 | Software engineering  | Software engineering discussions
(2 rows)
```

假设我们想要从表 t_categories 中提取值并用它们来更新表 categories 的值；以下是我们想要达到的结果集：

| pk | title | description |
|----+-----------------------------------+-----------------------------------|
| 1 | Database | Database related discussions |
| 2 | Unix | Unix and Linux discussions |
| 3 | Programming Languages | All about programming languages |
| 4 | Machine Learning | Machine Learning discussions |
| 5 | Software engineering | Software engineering discussions |

我们必须执行的查询是：

```
forumdb=>update categories c set title=t.title,description=t.description
from t_categories t where c.pk=t.pk;
UPDATE 2

forumdb=> select * from categories;
pk | title                | description
----+-----------------------------------+-------------------------------
1  | Database            | Database related discussions
2  | Unix                | Unix and Linux discussions
3  | Programming Languages | All about programming languages
4  | Machine Learning    | Machine Learning discussions
5  | Software engineering| Software engineering discussions
(5 rows)
```

在此查询中，PostgreSQL 能够使用来自表 t_categories 且在 pk 字段上匹配的数据来更新 categories 表的 title 和 description 字段；当我们谈论 merge 语句时，我们将看到另一种达到相同目标的方法。

## MERGE

从 PostgreSQL 15 开始，我们可以通过使用 MERGE 语句来实现我们在上一节中实现的目标；最好使用 MERGE 语句，因为它存在于 SQL 2003 ANSI 中。

现在，让我们从 categories 表的之前值开始：

```
forumdb=> select * from categories;
 pk |      title      |      description
----+---------------+-------------------------------
 1 | Database      | Database related discussions
 2 | Unix          | Unix and Linux discussions
 3 | Programming Languages | All about programming languages
 4 | Machine Learning      | Machine Learning discussions
 5 | Software engineering  | Software engineering discussions
(5 rows)
```

然后让我们创建另一个数据集，其中包含我们想要应用于 categories 表的一些更改：

```
forumdb=> create temp table new_data as select * from categories limit 0;
SELECT 0

forumdb=> insert into new_data (pk,title,description) values (1,'Database Discussions','Database discussions'),(2,'Unix/Linux discussion','Unix and Linux discussions');
INSERT 0 2

forumdb=> select * from new_data;
 pk |      title      |      description
----+---------------+-------------------------------
 1 | Database Discussions | Database discussions
 2 | Unix/Linux discussion | Unix and Linux discussions
(2 rows)
```

现在我们想要实现的目标是将两个数据集按如下所示合并：

```
1 | Database Discussions   | Database discussions
2 | Unix/Linux discussion  | Unix and Linux discussions
3 | Programming Languages  | All about programming languages
4 | Machine Learning       | Machine Learning discussions
5 | Software engineering   | Software engineering discussions
```

我们必须执行以达到此目标的查询是：

```
```sql
forumdb=> merge into categories c
using new_data n on c.pk=n.pk
when matched then
    update set title=n.title,description=n.description
when not matched then
    insert (pk,title,description)
    OVERRIDING SYSTEM VALUE values (n.pk,n.title,n.description);
MERGE 2

forumdb=> select * from categories order by 1;
 pk |         title         |             description
----+-----------------------+----------------------------------
  1 | Database Discussions  | Database discussions
  2 | Unix/Linux discussion | Unix and Linux discussions
  3 | Programming Languages | All about programming languages
  4 | Machine Learning      | Machine Learning discussions
  5 | Software engineering  | Software engineering discussions
(5 rows)
```
```

上面的查询检查 new_data 表的 PK 字段的值与 categories 表的字段值之间是否存在匹配。如果存在匹配，则执行 UPDATE；否则执行 INSERT。OVERRIDING SYSTEM VALUE 子句用于在 INSERT 语句中，我们还指定了从 new_data 表中插入 PK 字段的值的插入，并且由于 categories 表中的 PK 字段定义为 GENERATED ALWAYS，如果没有 OVERRIDING SYSTEM VALUE 子句，PostgreSQL 将生成错误。

## 探索 UPDATE ... RETURNING

与 INSERT 语句一样，update 语句也有添加 RETURNING 关键字的可能性。update 语句的工作方式与 INSERT 语句相同：

```
```sql
forumdb=> update categories set title='A.I' where pk=4 returning
pk,title,description;
 pk | title |           description
----+-------+------------------------------
  4 | A.I   | Machine Learning discussions
(1 row)
```
```

## 探索 DELETE ... RETURNING

正如我们所见，update 语句与 INSERT 语句一样，有可能添加 RETURNING 关键字；此功能也适用于 delete 语句：

```
forumdb=> delete from t_categories where pk=4 returning
pk,title,description;
 pk |      title      |        description
----+---------------+-----------------------------
  4 | Machine Learning | Machine Learning discussions
(1 row)
DELETE 1
```

在下一节中，我们将讨论 CTE，这是一种高级方法来返回和修改数据。

## 探索 CTEs

在本节中，我们将讨论 CTEs。本节将分为三个部分。首先，我们将讨论 CTEs 的概念；其次，我们将讨论从 PostgreSQL 12 开始 CTEs 的实现方式；最后，我们将探索一些使用 CTEs 的示例。

### CTE 概念

CTE，或公共表表达式，是从 SQL 语句中获取的临时结果。此语句可以包含 SELECT、INSERT、UPDATE 或 DELETE 指令。CTE 的生命周期等于查询的生命周期。以下是 CTE 定义的示例：

```
WITH cte_name (column_list) AS (
  CTE_query_definition
)
statement;
```

例如，如果我们想要创建一个包含作者 enrico_pirozzi 编写的所有帖子的临时数据集，我们必须编写如下内容：

```
forumdb=> with posts_author_1 as
(select p.* from posts p
inner join users u on p.author=u.pk
where username='enrico_pirozzi')
select pk,title from posts_author_1;
```

