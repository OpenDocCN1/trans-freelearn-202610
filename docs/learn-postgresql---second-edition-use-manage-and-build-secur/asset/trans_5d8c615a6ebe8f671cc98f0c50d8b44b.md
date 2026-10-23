

在这个服务器上，时区设置为 UTC；如果我们只想在此会话中修改该值，我们必须执行以下查询：

```
forumdb=> set timezone='CET';
SET
```

现在，时区已设置为 CET：

```
forumdb=> show timezone;
-[ RECORD 1 ]-
TimeZone | CET
```

现在，如果我们再次执行之前执行的查询，我们会看到时区字段的值已发生变化：

```
forumdb=> select * from new_posts ;
-[ RECORD 1 ]+------------------------------
pk           | 5
title        | Indexing PostgreSQL
created_on_t | 2023-01-23 16:21:55.747463+01
created_on_nt | 2023-01-23 15:21:55.747463
-[ RECORD 2 ]+------------------------------
pk           | 6
title        | Indexing Mysql
created_on_t | 2023-01-23 16:22:02.38953+01
created_on_nt | 2023-01-23 15:22:02.38953
-[ RECORD 3 ]+------------------------------
pk           | 7
title        | A view of Data types in C++
created_on_t | 2023-01-23 16:26:21.367814+01
created_on_nt | 2023-01-23 15:26:21.367814
```

这展示了带时区的时间戳与不带时区的时间戳之间的区别。有关日期和时间主题的更多信息，请参阅 https://www.postgresql.org/docs/current/datatype-datetime.html 上的官方文档。

## NoSQL 数据类型

在本节中，我们将介绍 PostgreSQL 中存在的 NoSQL 数据类型。由于本书并非专门针对 NoSQL，我们只会快速浏览一下。

PostgreSQL 处理以下 NoSQL 数据类型：

- hstore
- xml
- json/jsonb

我们现在将讨论 hstore 和 json。

### hstore 数据类型

hstore 是 PostgreSQL 中实现的首个 NoSQL 数据类型。此数据类型用于在单个值中存储键值对。在使用 hstore 数据类型之前，我们需要在服务器上启用 hstore 扩展：

```
forumdb=> create extension hstore ;
CREATE EXTENSION
```

让我们通过一个示例来看如何使用 hstore 数据类型。假设我们想要显示所有帖子及其用户名和分类：

```
forumdb=> select p.pk,p.title,u.username,c.title as category
from posts p
inner join users u on p.author=u.pk
left join categories c on p.category=c.pk
order by 1;
-[ RECORD 1 ]---------------------------------
pk      | 5
title   | Indexing PostgreSQL
username | luca_ferrari
category | Database
-[ RECORD 2 ]---------------------------------
pk      | 6
title   | Indexing Mysql
username | luca_ferrari
category | Database
-[ RECORD 3 ]---------------------------------
pk      | 7
title   | A view of Data types in C++
username | enrico_pirozzi
category | Programming Languages
```

现在假设 posts、users 和 categories 表都是巨大的表，我们希望将用户名和分类的全部信息存储在 posts 表内的单个字段中。如果我们能做到这一点，就不再需要连接三个巨大的表。在这种情况下，hstore 可以帮助我们：

```
forumdb=> select p.pk,p.title,hstore(ARRAY['username',u.username,'category',c.title]) as options
from posts p
inner join users u on p.author=u.pk
left join categories c on p.category=c.pk
order by 1;
-[ RECORD 1 ]--------------------------
pk      | 5
title   | Indexing PostgreSQL
options | "category"=>"Database", "username"=>"luca_ferrari"
-[ RECORD 2 ]--------------------------
pk      | 6
title   | Indexing Mysql
options | "category"=>"Database", "username"=>"luca_ferrari"
-[ RECORD 3 ]--------------------------
pk      | 7
title   | A view of Data types in C++
options | "category"=>"Programming Languages", "username"=>"enrico_pirozzi"
```

前面的查询首先将用户名和分类字段的值放入一个数组中，然后将其转换为 hstore。现在，如果我们想将数据存储在名为 posts_options 的新表中，我们必须执行类似以下操作：

```
forumdb=> create table posts_options as
select p.pk,p.title,hstore(ARRAY['username',u.username,'category',c.title]) as options
from posts p
inner join users u on p.author=u.pk
left join categories c on p.category=c.pk
order by 1;
SELECT 3
```

我们现在有一个结构如下的新表：

```
forumdb=> \d posts_options

               Table "forum.posts_options"

 Column |  Type   | Collation | Nullable | Default
--------+---------+-----------+----------+---------
pk      | integer |           |          |
title   | text    |           |          |
options | hstore  |           |          |
```

接下来，假设我们想要搜索所有 category = 'Database' 的记录。我们必须执行以下操作：

```
forumdb=> select * from posts_options where options->'category' ='Database';
-[ RECORD 1 ]---------------------------------
pk      | 5
title   | Indexing PostgreSQL
options | "category"=>"Database", "username"=>"luca_ferrari"
-[ RECORD 2 ]---------------------------------
pk      | 6
title   | Indexing Mysql
options | "category"=>"Database", "username"=>"luca_ferrari"
```

由于 hstore 以及 json/jsonb 数据类型都不是结构化数据类型，我们可以在不先定义的情况下插入任何其他键值——例如，我们可以这样做：

```
forumdb=> insert into posts_options (pk,title,options) values (7,'my last post','"enabled"=>"false"');
INSERT 0 1
```

对整个表的选择结果如下：

```
forumdb=>  select * from posts_options;
-[ RECORD 1 ]---------------------------------
pk      | 5
title   | Indexing PostgreSQL
options | "category"=>"Database", "username"=>"luca_ferrari"
-[ RECORD 2 ]---------------------------------
pk      | 6
title  | Indexing Mysql
options | "category"=>"Database", "username"=>"luca_ferrari"
-[ RECORD 3 ]--------------------------------
pk      | 7
title   | A view of Data types in C++
options | "category"=>"Programming Languages", "username"=>"enrico_pirozzi"
-[ RECORD 4 ]--------------------------------
pk      | 7
title   | my last post
options | "enabled"=>"false"
```

正如本节开头所说，NoSQL 不是本书的主题，但值得简要回顾一下。有关 hstore 数据类型的更多信息，请参阅 https://www.postgresql.org/docs/current/hstore.html 上的官方文档。

### JSON 数据类型

在本节中，我们将简要介绍 JSON 数据类型。JSON 代表 JavaScript Object Notation。JSON 是一种开放标准格式，由键值对组成。PostgreSQL 原生支持 JSON 数据类型。它提供了许多用于操作 JSON 数据的函数和运算符。PostgreSQL 除了 json 数据类型外，还支持 jsonb 数据类型。这两种数据类型的区别在于，前者内部以文本表示，而后者内部以二进制且可索引的方式表示。让我们通过一个示例来看如何使用 json/jsonb 数据类型。

假设我们想要显示 forumdb 数据库中所有的帖子和标签。以经典的关系型 SQL 方式工作，我们应该写类似以下内容：

```
forumdb=> \x
Expanded display is off.
forumdb=> select p.pk,p.title,t.tag
from posts p
left join j_posts_tags jpt on p.pk=jpt.post_pk
left join tags t on jpt.tag_pk=t.pk
order by 1;
pk |          title          |   tag
----+--------------------------------+----------------
5 | Indexing PostgreSQL | Operating Systems
5 | Indexing PostgreSQL | Database
6 | Indexing Mysql | Database
6 | Indexing Mysql | Operating Systems
7 | A view of Data types in C++ | Database
(5 rows)
```

现在假设我们想要得到如下结果：

| pk | title | tag |
| --- | --- | --- |
| 5 | Indexing PostgreSQL | Operating Systems,Database |
| 6 | Indexing PostgreSQL | Database,Operating Systems |
| 7 | A view of Data types in C++ | Database |

以关系型方式，我们必须使用前两个字段聚合数据并执行类似以下操作：

```
forumdb=> \x
Expanded display is on.
forumdb=> select p.pk,p.title,string_agg(t.tag,',') as tag
from posts p
left join j_posts_tags jpt on p.pk=jpt.post_pk
left join tags t on jpt.tag_pk=t.pk
group by 1,2
order by 1;
-[ RECORD 1 ]----------------------
pk | 5
title | Indexing PostgreSQL
tag | Operating Systems,Database
-[ RECORD 2 ]----------------------
pk | 6
title | Indexing Mysql
tag | Database, Operating Systems
-[ RECORD 3 ]----------------------
pk | 7
title | A view of Data types in C++
tag | Database
```

