## Server-Side Programming

在之前的章节中，我们学习了如何执行 SQL 查询。我们从编写简单的查询开始，然后转向编写更复杂的查询；我们学习了如何以传统方式使用聚合，并在第 5 章 高级语句 中讨论了窗口函数，这是编写聚合的另一种方式。在本章中，我们将服务器端编程添加到这份技能清单中。服务器端编程在许多情况下很有用，因为它将编程逻辑从客户端移至数据库端。例如，我们可以将一个在应用程序的不同位置多次编写的函数移至服务器内部，使其只需编写一次，这意味着在修改时，我们只需修改一个函数。本章中，我们还将了解 PostgreSQL 如何管理不同的服务器端编程语言，并将看到如果需要处理从表中提取的大量数据，服务器端编程会非常有用。我们将讨论我们编写的所有函数都可以在任何 SQL 语句中调用这一事实。我们还将看到在某些情况下，对于某些类型的函数，也可以创建函数的索引。

服务器端编程的另一个特点是可以定义自定义数据。在本章中，我们将看一些示例。

简单来说，本章将讨论以下内容：

+   - 探索数据类型
+   - 探索函数和语言
+   - NoSQL 数据类型





## 技术要求

开始之前，请记住启动名为 chapter_07 的 Docker 容器，如下所示：

```
$ bash run-pg-docker.sh chapter_07
postgres@learn_postgresql:~$ psql -U forum forumdb
```

## 探索数据类型

作为用户，我们已经有机会体验服务器端函数的强大和多功能性——例如，在第 5 章《高级语句》中，我们使用了类似以下的查询：

```
forumdb=> select * from categories where upper(title) like 'A%';
pk | title | description
----+-------+-----------------------------
 4 | A.I | Machine Learning discussions
(1 row)
```

在这段代码中，upper 函数是一个服务器端函数；该函数将字符串的所有字符转换为大写。在本章中，我们将获得编写类似我们在前述查询中调用的 upper 函数的知识。

在本节中，我们将讨论数据类型。我们将简要提及 PostgreSQL 管理的标准类型以及如何创建新的类型。

## 可扩展性的概念

什么是可扩展性？可扩展性是 PostgreSQL 扩展其功能和数据类型的能力。可扩展性是一个极其有用的 PostgreSQL 功能，因为它使我们能够拥有在基础系统中不存在的数据类型、函数和功能索引。在本章中，我们将涵盖数据类型的扩展，以及新函数的添加。

## 标准数据类型

在之前的章节中，即使并不明显，我们已经使用了标准数据类型。这是在我们学习如何使用数据定义语言（DDL）命令时发生的。然而，我们现在将更深入地研究这个主题。以下是使用最频繁的数据类型的简短列表：

- Boolean 类型
- 数字类型
- 字符类型

4. 现在，如果我们想搜索所有 user_on_line 字段设置为 true 的记录，我们必须执行以下操作：

```
forumdb=> \x
Expanded display is on.
forumdb=> select * from users where user_on_line = true;
-[ RECORD 1 ]+---------------------
pk            | 1
username      | luca_ferrari
gecos         |
email         | luca@pgtraining.com
user_on_line  | t
```

5. 如果我们想搜索所有 user_on_line 字段设置为 NULL 的记录，如我们在第 4 章《基本语句》中所见，我们必须执行以下操作：

```
forumdb=> select * from users where user_on_line is NULL;
-[ RECORD 1 ]+---------------------
pk            | 2
username      | enrico_pirozzi
gecos         |
email         | enrico@pgtraining.com
user_on_line  |
-[ RECORD 2 ]+---------------------
pk            | 3
username      | newuser
gecos         |
email         | newuser@pgtraining.com
user_on_line  |
```

因此，我们已经探索了 Boolean 数据类型。

## 数字数据类型

PostgreSQL 支持几种类型的数字数据类型；最常用的如下：

- integer 或 int4（4 字节整数）
- bigint 或 int8（8 字节整数）
- real（4 字节可变精度，不精确，6 位小数精度）
- double precision（8 字节可变精度，不精确，15 位小数精度）
- numeric（precision, scale），其中 numeric 的精度是整个数字中有效数字的总计数，而 numeric 的小数位数是小数部分中十进制数字的计数。例如，5.827 的精度为 4，小数位数为 3。

现在，我们将在接下来的部分中查看每种类型的一些简要示例。

## 整数类型

正如我们在这里看到的，如果我们将一个数字转换为整数类型，例如 integer 或 bigint，PostgreSQL 会对输入数字进行截断值：

```
forumdb=> \x
Expanded display is off.
forumdb=> select 1.123456789::integer as my_field;
 my_field
----------
        1
(1 row)

forumdb=> select 1.123456789::int4 as my_field;
 my_field
----------
        1
(1 row)
forumdb=> select 1.123456789::bigint as my_field;
 my_field
----------
        1
(1 row)

forumdb=> select 1.123456789::int8 as my_field;
 my_field
----------
        1
(1 row)
```

```
1.1
(1 row)

forumdb=> select 1.123456789::numeric(10,5) as my_field;
my_field
----------
1.12346
(1 row)

forumdb=> select 1.123456789::numeric(10,9) as my_field;
my_field
------------
1.123456789
(1 row)
```

正如我们从这里显示的示例中看到的，我们决定小数位数应该有多少位。

但是如果我们执行类似以下的操作会怎样？

```
forumdb=> select 1.123456789::numeric(10,11) as my_field;
ERROR:  numeric field overflow
DETAIL:  A field with precision 10, scale 11 must round to an absolute value less than 10^-1.
```

结果是一个错误。这是因为数据类型被定义为精度值等于 10 的 numeric 类型，所以我们不能使小数位数参数等于或大于精度值。

同样，下一个示例也会产生错误：

```
forumdb=> select 1.123456789::numeric(10,10) as my_field;
ERROR:  numeric field overflow
DETAIL:  A field with precision 10, scale 10 must round to an absolute value less than 1.
```

在前面的示例中，查询生成错误是因为小数位数为 10，意味着我们应该有 10 位数字，但我们总共有 11 位数字：

| Digits | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|---|---|---|---|---|---|---|---|---|---|---|---|
|  | 1 | . | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |

然而，如果在我们的数字中没有第一位数字，查询将可以工作：

```
forumdb=> select 0.123456789::numeric(10,10) as my_field;
my_field
-------------
0.1234567890
(1 row)
```

现在让我们回到上一段提供的错误求和示例，并使用 numeric 类型重复它：

```
forumdb=> select sum(0.1::numeric(2,2)) from generate_series(1,10);
sum
-----
1.00
(1 row)
```

正如我们所见，现在求和的值是正确的；因此，表示货币的正确方法是使用 numeric 数据类型。

因此，我们已经了解了各种数字数据类型的全部内容。

## 字符数据类型

PostgreSQL 中使用最频繁的字符数据类型如下：

- character(n)/char(n)（定长，空白填充）
- character varying(n)/varchar(n)（变长，有限制）
- varchar/text（变长，无限制）

现在，我们将查看一些示例，看看 PostgreSQL 如何管理这些类型的数据。

## 固定长度的字符类型

我们将使用以下示例来检查它们的工作方式：

1. 首先创建一个新的测试表：

```
forumdb=> create table new_tags (
pk integer not null primary key,
tag char(10)
);
CREATE TABLE
```

在前面的代码中，我们创建了一个名为 new_tags 的新表，其中包含一个 char(10) 字段名 tag。

2. 现在，让我们添加一些记录并看看 PostgreSQL 的行为：

```
forumdb=> insert into new_tags values (1,'first tag');
INSERT 0 1
forumdb=> insert into new_tags values (2,'tag');
INSERT 0 1
```

为了继续我们的分析，我们必须引入两个新函数：

- length(p)：这会计算字符数，其中 p 是输入参数且为字符串
- octet_length(p)：这会计算字节数，其中 p 是输入参数且为字符串

3. 让我们执行以下查询：

```
forumdb=> \x
Expanded display is on.
forumdb=> select pk,tag,length(tag),octet_length(tag),char_length(tag) from new_tags;
-[ RECORD 1 ]----------
pk      | 1
tag     | first tag
length  | 9
octet_length | 10
char_length  | 9
-[ RECORD 2 ]----------
pk      | 2
tag     | tag
length  | 3
octet_length | 10
char_length  | 3
```

正如我们所见，字段内部占用的总长度始终为 10；即使输入的字符数不同也是如此。这是因为我们将字段定义为 char(10)，固定长度为 10，因此即使我们插入一个较短的字符串，10 与字符串实际字符数之间的差异也将用空白字符填充。





## 具有长度限制的可变长度字符数据类型

在本节中，我们将重复上一节中使用过的同一个示例，但这次，我们将使用 varchar(10) 数据类型作为 tag 字段：

- 1. 让我们重新创建 new_tags 表：
- 2. 然后，让我们插入一些数据：
- 3. 现在，如果我们重复之前的相同查询，我们将得到以下结果：

```
forumdb=> drop table if exists new_tags;
DROP TABLE

forumdb=> create table new_tags (
pk integer not null primary key,
tag varchar(10)
);
CREATE TABLE
```

```
forumdb=> insert into new_tags values (1,'first tag');
INSERT 0 1

forumdb=> insert into new_tags values (2,'tag');
INSERT 0 1
```

```
forumdb=> \x
Expanded display is off.
forumdb=> select pk,tag,length(tag),octet_length(tag) from new_tags ;
 pk |   tag    | length | octet_length
----+----------+--------+--------------
  1 | first tag |      9 |            9
  2 | tag      |      3 |            3
(2 rows)
```

正如我们所见，这次，实际内部大小和字符串中的字符数是相同的。

在前面的示例中，我们可以看到 text 数据类型与我们之前看到的 varchar(n) 数据类型完全相同。text 与 varchar(n) 之间的唯一区别是 text 类型没有大小限制。需要注意的是，在前面的查询中，我们使用了 substring 函数。substring 函数从 from 参数开始截取字符串的 n 个字符；例如，如果我们写 substring(tag from 0 for 20)，它意味着我们希望将 tag 字符串的前 20 个字符作为输出。

至此，我们已经涵盖了所有 char 数据类型。

## 日期/时间戳数据类型

在本节中，我们将讨论如何在 PostgreSQL 中存储日期和时间。PostgreSQL 支持日期和时间以及日期时间的组合（timestamp）。PostgreSQL 管理带时区设置和不带时区设置的小时，如官方文档所述（https://www.postgresql.org/docs/current/datatype-datetime.html）。

> PostgreSQL 支持完整的 SQL 日期和时间类型。日期是按照格里高利历计算的。

### 日期数据类型

管理日期对于开发者来说通常会成为一个难题。这是因为日期的表示方式取决于我们需要存储数据的国家——例如，美国的方式是月/日/年，而欧洲的格式是日/月/年。PostgreSQL 通过提供必要的工具来帮助我们最好地解决这个问题，如下所示：

- 1. 我们要做的第一件事是查看 PostgreSQL 内部如何存储日期。为此，我们必须执行以下查询：

```
forumdb=> \x
Expanded display is on.
forumdb=> select * from pg_settings where name ='DateStyle';
-[ RECORD 1 ]---+--------------------------------------------------
--
name            | DateStyle
setting         | ISO, MDY
[...] 
sourcefile      |
```

```
sourceline  |
pending_restart | f
```

首先，让我们看一下 pg_settings 视图。使用 pg_settings 视图，我们可以查看 postgresql.conf 配置文件中设置的<|message|> 参数。在前面的结果中，我们可以看到日期显示的配置为 MDY（月/日/年）。如果我们想全局更改此参数，我们必须编辑 postgresql.conf 文件。

- 2. 在 Debian 或基于 Debian 的服务器上，我们可以按如下方式编辑文件：

```
root@pgdev:~# vim /etc/postgresql/16/main/postgresql.conf
```

- 3. 然后，我们必须修改以下部分：

```
#Locale and Formatting
datestyle = 'iso, mdy'
```

- 4. 更改此参数后，在对 pg_settings 的查询中，context 参数为 'user'；我们只需要重新加载服务器。在这种情况下，无需重启：

```
root@pgdev:~# service postgresql reload
[ ok ] Reloading postgresql configuration (via systemd):
postgresql.service.
```

有关 pg_settings 视图的更多信息，我们建议访问 https://www.postgresql.org/docs/current/view-pg-settings.html。

- 5. 我们已经了解了日期显示的内部参数，现在，让我们看看如何插入、更新和显示日期。如果我们知道 date-style 参数的值，将字符串转换为日期的 PostgreSQL 方法如下：

```
forumdb=> \x
Expanded display is off.
forumdb=> select '12-31-2020'::date;
    date
------------
2020-12-31
(1 row)
```

这种方法很简单，但并不特别用户友好。管理日期的最佳方式是使用 PostgreSQL 为我们提供的一些函数。

- 6. 我们要谈到的第一个函数是 to_date() 函数。to_date() 函数将给定的字符串转换为日期。to_date() 函数的语法如下：

```
forumdb=> select to_date('31/12/2020','dd/mm/yyyy');
to_date
-----------
2020-12-31
(1 row)
```

to_date() 函数接受两个字符串参数。第一个参数包含我们想要转换为日期的值。第二个参数是日期的模式。to_date() 函数返回一个日期值。

- 7. 现在，让我们回到 posts 表并执行此查询：

```
forumdb=> \x
Expanded display is on.
forumdb=> select pk,title,created_on from posts;
-[ RECORD 1 ]---------------------------------
pk         | 5
title      | Indexing PostgreSQL
created_on | 2023-01-23 15:21:55.747463+00
-[ RECORD 2 ]---------------------------------
pk         | 6
title      | Indexing Mysql
created_on | 2023-01-23 15:22:02.38953+00
-[ RECORD 3 ]---------------------------------
pk         | 7
title      | A view of  Data types in C++
created_on | 2023-01-23 15:26:21.367814+00
```

如果从未有人将这些值输入到表中，我们怎么会有日期/时间组合（时间戳）呢？这是可能的，因为 posts 表是按如下方式创建的：

```
forumdb=> \d posts;
Table "public.posts"
Column           | Type                | [...] | Default
------------------+--------------------------------+--------+--------
```

```
pk | integer | | [...] 
title | text | | 
[...] 
created_on | timestamp with time zone | | CURRENT_TIMESTAMP
```

正如我们所见，created_on 字段的默认值是 CURRENT_TIMESTAMP，这意味着如果没有插入值，服务器的当前时间戳将被插入。假设现在我们想以不同的格式显示日期——例如，以欧洲格式显示，created_on: 03-01-2020。

- 8. 为了达到这个目标，我们必须使用另一个内置函数，to_char 函数：

```
forumdb=> select pk,title,to_char(created_on,'dd-mm-yyyy') as created_on
from posts;
-[ RECORD 1 ]--------------------------------
pk | 5
title | Indexing PostgreSQL
created_on | 23-01-2023
-[ RECORD 2 ]--------------------------------
pk | 6
title | Indexing Mysql
created_on | 23-01-2023
-[ RECORD 3 ]--------------------------------
pk | 7
title | A view of Data types in C++
created_on | 23-01-2023
```

如图所示，to_char() 函数是 to_date() 函数的逆函数。

### 时间戳数据类型

PostgreSQL 可以管理带时区和不带时区的日期和时间。我们可以使用 timestamp 数据类型存储日期和时间。在 PostgreSQL 中，有一个名为 timestamp with time zone 的数据类型用于以时区显示日期和时间，还有一个名为 timestamp without time zone 的数据类型用于存储不带时区的日期和时间。

让我们通过一些示例来了解。首先，让我们创建一个新表：

```
forumdb=> create table new_posts as select pk,title,created_on::timestamp with time zone as created_on_t, created_on::timestamp without time zone as
```

```
create_on_nt from posts;
SELECT 3
```

我们刚刚创建了一个名为 new_posts 的新表，其结构如下：

```
forumdb=# \d new_posts;
 Table "public.new_posts"
 Column | Type | [...] 
 ---------------+-------------------------------+---------
 pk | integer |
 title | text |
 created_on_t | timestamp with time zone |
 create_on_nt | timestamp without time zone |
```

此表现在对 create_on_t（带时区的时间戳）字段和 created_on_nt（不带时区的时间戳）字段具有相同的值，如下所示：

```
forumdb=> select * from new_posts ;
-[ RECORD 1 ]-----------------------------------
pk | 5
title | Indexing PostgreSQL
created_on_t | 2023-01-23 15:21:55.747463+00
create_on_nt | 2023-01-23 15:21:55.747463
-[ RECORD 2 ]-----------------------------------
pk | 6
title | Indexing Mysql
created_on_t | 2023-01-23 15:22:02.38953+00
create_on_nt | 2023-01-23 15:22:02.38953
-[ RECORD 3 ]-----------------------------------
pk | 7
title | A view of Data types in C++
created_on_t | 2023-01-23 15:26:21.367814+00
create_on_nt | 2023-01-23 15:26:21.367814
```

现在，让我们引入一个 PostgreSQL 环境变量，称为 timezone 变量。此变量告诉我们时区的当前值：

```
forumdb=> show timezone;
-[ RECORD 1 ]-----
TimeZone | Etc/UTC
```





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



现在，想象一下我们想要生成一个简单的 JSON 结构；我们可以执行以下查询：

```
forumdb=> select row_to_json(q) as json_data from (
  select p.pk,p.title,string_agg(t.tag,',') as tag
  from posts p
  left join j_posts_tags jpt on p.pk=jpt.post_pk
  left join tags t on jpt.tag_pk=t.pk
  group by 1,2 order by 1) q;
-[ RECORD 1 ]-----------------------------
json_data | {"pk":5,"title":"Indexing PostgreSQL","tag":"Operating Systems,Database"}
-[ RECORD 2 ]-----------------------------
json_data | {"pk":6,"title":"Indexing Mysql","tag":"Database,Operating Systems"}
-[ RECORD 3 ]-----------------------------
json_data | {"pk":7,"title":"A view of  Data types in C++","tag":"Database"}
```

正如我们所见，通过一个简单的查询，就可以从经典的 SQL 表示切换到 NoSQL 表示。现在，让我们创建一个名为 post_json 的新表。这个表只包含一个名为 jsondata 的 jsonb 字段：

```
forumdb=> create table post_json (jsondata jsonb);
CREATE TABLE
forumdb=> \d post_json
          Table "forum.post_json"
 Column  |  Type   | Collation | Nullable | Default
---------+---------+-----------+----------+--------
jsondata | jsonb   |           |          |
```

现在，让我们向 post_json 表中插入一些数据：

```
forumdb=> insert into post_json(jsondata)
select row_to_json(q) as json_data from (
  select p.pk,p.title,string_agg(t.tag,',') as tag
  from posts p
left join j_posts_tags jpt on p.pk=jpt.post_pk
left join tags t on jpt.tag_pk=t.pk
group by 1,2 order by 1) Q;
INSERT 0 3
```

现在，post_json 表包含以下记录：

```
forumdb=> select jsonb_pretty(jsondata) from post_json;
-[ RECORD 1 ]+-------------------------
jsonb_pretty | {
             |   "pk": 5,
             |   "tag": "Operating Systems,Database",
             |   "title": "Indexing PostgreSQL"
             | }
-[ RECORD 2 ]+-------------------------
jsonb_pretty | {
             |   "pk": 6,
             |   "tag": "Database,Operating Systems",
             |   "title": "Indexing Mysql"
             | }
-[ RECORD 3 ]+-------------------------
jsonb_pretty | {
             |   "pk": 7,
             |   "tag": "Database",
             |   "title": "A view of  Data types in C++"
             | }
```

如果我们想要搜索所有 tag = "Database" 的数据，可以使用 `@>` jsonb 运算符。该运算符检查左侧 JSON 值是否在顶层包含右侧 JSON 路径/值条目；以下查询可以实现这种搜索：

```
forumdb=> select jsonb_pretty(jsondata) from post_json where jsondata @> '{"tag":"Database"}';

-[ RECORD 1 ]+-------------------------
jsonb_pretty | {
             |   "pk": 7,
             |   "tag": "Database",
             |   "title": "A view of  Data types in C++"+
             | }
```

我们刚才所写的只是通过 NoSQL 数据模型可以实现的功能的一小部分。JSON 在处理大型表以及需要一种能够最大限度减少研究阶段连接次数的数据结构时被广泛使用。关于 NoSQL 世界的详细讨论超出了本书的范围，但我们想简要描述一下 PostgreSQL 在处理非结构化数据方面有多么强大。如需更多信息，请查看官方文档：https://www.postgresql.org/docs/current/functions-json.html。

在理解了什么是数据类型以及 PostgreSQL 中可以使用哪些数据类型之后，下一节我们将看到如何在函数中使用数据类型。

## 探索函数和语言

PostgreSQL 能够执行服务器端代码。有许多方法可以为 PostgreSQL 提供要执行的代码。例如，用户可以用不同的编程语言创建函数。PostgreSQL 支持的主要语言如下：

- SQL
- PL/pgSQL
- C

这些列出的语言是内置语言；PostgreSQL 还可以管理其他语言，但在使用它们之前，我们需要在系统中安装它们。其他一些受支持的语言如下：

- PL/Python
- PL/Perl
- PL/tcl
- PL/Java

在本节中，我们将讨论 SQL 和 PL/pgSQL 函数。

### 函数

定义函数的命令结构如下：

```
CREATE FUNCTION function_name(p1 type, p2 type,p3 type, ....., pn type)
RETURNS type AS
BEGIN
-- function logic
END;
LANGUAGE language_name
```

以下步骤始终适用于我们要创建的任何类型的函数：

- 1. 在 CREATE FUNCTION 关键字之后指定函数名称。
- 2. 列出以逗号分隔的参数列表。
- 3. 在 RETURNS 关键字之后指定返回数据类型。
- 4. 对于 PL/pgSQL 语言，在 BEGIN 和 END 块之间放置一些代码。
- 5. 对于 PL/pgSQL 语言，函数必须以 END 关键字结尾，后跟分号。
- 6. 定义编写函数所用的语言——例如 sql、plpgsql、plperl、plpython 等。

这是本章后面将参考的基本方案；在某些特定情况下，此方案可能会有细微变化。

### SQL 函数

SQL 函数是在 PostgreSQL 中编写函数的最简单方式，我们可以在其中使用任何 SQL 命令。

### 基本函数

本节将展示如何迈出进入 SQL 函数世界的第一步。例如，以下函数对两个数字进行求和：

```
forumdb=> CREATE OR REPLACE FUNCTION my_sum(x integer, y integer) RETURNS integer AS $$
    SELECT x + y;
$$ LANGUAGE SQL;
CREATE FUNCTION

forumdb=> select my_sum(1,2);
 my_sum
--------
3
(1 row)
```

正如我们在前面示例中所看到的，函数代码放在 `$$` 之间；我们可以将 `$$` 视为标签。可以使用不带任何 FROM 子句的 SELECT 语句来调用该函数。SQL 函数的参数可以在函数体中使用数字（旧方式）或其名称（新方式）来引用。例如，我们可以用以下方式编写同一个函数：

```
CREATE OR REPLACE FUNCTION my_sum(integer, integer) RETURNS integer AS $$
SELECT $1 + $2;
$$ LANGUAGE SQL;
```

在前面的函数中，我们可以看到在函数内部引用参数的旧方式。在旧方式中，参数按位置引用，因此 `$1` 对应函数的第一个参数，`$2` 对应第二个参数，依此类推。在 SQL 函数的代码中，我们可以使用所有 SQL 命令，包括前面章节中介绍过的那些命令。

### SQL 函数返回元素集合

在本节中，我们将了解如何创建一个返回数据类型结果集的 SQL 函数。例如，假设我们想编写一个函数，该函数接受 p_title 作为参数，删除所有 title=p_title 的记录，并返回所有已删除记录的主键。以下函数可以实现这一点：

```
forumdb=> CREATE OR REPLACE FUNCTION delete_posts(p_title text) returns setof integer as $$
delete from posts where title=p_title returning pk;
$$
LANGUAGE SQL;
CREATE FUNCTION
```

这是我们调用 delete_posts 函数之前的情况：

```
forumdb=> select pk,title from posts order by pk;
pk | title
----+--------------------------------
5 | Indexing PostgreSQL
6 | Indexing Mysql
7 | A view of Data types in C++
(3 rows)
```

现在，假设我们想要删除 title 字段等于 "A view of Data types in C++" 的记录。表 posts 以 pk 字段作为主键，对于记录 "A view of Data types in C++"，pk 的值等于 7；因此，首先让我们从 j_posts_tags 表中删除 post_pk=7 的记录。这是因为有一个外键将 posts 表和 j_posts_tags 表关联起来：

```
forumdb=> delete from j_posts_tags where post_pk = 7;
DELETE 1
```

现在让我们使用 "A view of  Data types in C++" 作为参数来调用 delete_posts 函数。这是我们调用 delete_posts 函数之后的情况：

```
forumdb=> select delete_posts('A view of  Data types in C++');
delete_posts
-------------
            7
(1 row)
forumdb=> select pk,title from posts order by pk;
pk |      title
----+---------------
 5 | Indexing PostgreSQL
 6 | Indexing Mysql
(2 rows)
```

在这个函数中，我们引入了一种新的数据类型——setof 数据类型。setof 指令只是定义了一个数据类型的结果集。例如，delete_posts 函数被定义为返回一个整数集合，因此其结果将是一个整数数据集。我们可以将 setof 指令用于任何类型的数据。

### SQL 函数返回表

在上一节中，我们看到了如何编写一个返回单一数据类型结果集的函数；然而，在某些情况下，我们可能需要函数返回多个字段的结果集。例如，让我们考虑与之前相同的函数，但这一次，我们希望返回 pk、title 对作为结果，因此我们的函数变成如下形式：

```
forumdb=> create or replace function delete_posts_table (p_title text)
returns table (ret_key integer,ret_title text) AS $$
delete from posts where title=p_title returning pk,title;
$$
```



```sql
language SQL;
CREATE FUNCTION
```

唯一的区别是，现在这个函数返回的是表类型；在表类型内部，我们需要指定字段的名称和类型。正如我们之前所见，在调用函数之前的情况是：

```sql
forumdb=> select pk, title from posts order by pk;
```

| pk | title |
|----|-------|
| 5 | Indexing PostgreSQL |
| 6 | Indexing Mysql |

(2 rows)

现在让我们插入一条新记录：

```sql
forumdb=> insert into posts(title, author, category) values ('My new post', 1, 1);
INSERT 0 1
```

现在让我们调用 delete_posts_table 函数。调用函数的正确方式是：

```sql
forumdb=> select * from delete_posts_table('My new post');
```

| ret_key | ret_title |
|---------|-----------|
| 9 | My new post |

(1 row)

调用函数之后的情况是：

```sql
forumdb=> select pk, title from posts order by pk;
```

| pk | title |
|----|-------|
| 5 | Indexing PostgreSQL |
| 6 | Indexing Mysql |

(2 rows)

返回表的函数可以被当作真正的表来处理，因为我们可以用它们配合 IN、EXISTS、JOIN 等选项使用。

## 多态 SQL 函数

在本节中，我们将简要谈谈多态 SQL 函数。

多态函数对 DBA 很有用，当我们需要编写一个能够处理不同类型数据的函数时。为了更好地理解多态函数，让我们从一个例子开始。假设我们想重现类似 Oracle NVL 函数的功能——换句话说，我们想创建一个函数，接受两个参数，如果第一个参数为 NULL，则用第二个参数替换第一个参数。问题是我们希望编写一个适用于所有数据类型（整数、实数、文本等）的单一函数。

下面的函数使这成为可能：

```sql
forumdb=> create or replace function nvl ( anyelement, anyelement) returns anyelement as $$
select coalesce($1, $2);
$$
language SQL;
CREATE FUNCTION
```

调用方法如下：

```sql
forumdb=> select nvl(NULL::int, 1);
```

| nvl |
|-----|
| 1 |

(1 row)

```sql
forumdb=> select nvl(''::text, 'n'::text);
```

| nvl |
|-----|
|

(1 row)

```sql
forumdb=> select nvl('a'::text, 'n'::text);
```

| nvl |
|-----|
| a   |

(1 row)

更多信息请参阅官方文档 https://www.postgresql.org/docs/current/extend-type-system.html。

## PL/pgSQL 函数

在本节中，我们将谈谈 PL/pgSQL 语言。PL/pgSQL 语言是 PostgreSQL 的默认内置过程语言。正如官方文档所述，PL/pgSQL 的设计目标是创建一个可加载的过程语言，能够做到以下几点：

- 可用于创建函数和触发器过程（我们将在下一章讨论触发器）。
- 添加新的控制结构。
- 向 SQL 语言添加新的数据类型。

它与 Oracle PL/SQL 非常相似，并支持以下功能：

- 变量声明
- 表达式
- 控制结构，如条件结构或循环结构
- 游标

### 初步概览

正如我们在 SQL 函数部分开头所见，PostgreSQL 中编写函数的原型如下：

```sql
CREATE FUNCTION function_name(p1 type, p2 type, p3 type, ....., pn type)
RETURNS type AS
BEGIN
-- function logic
END;
LANGUAGE language_name
```

现在，假设我们想使用 PL/pgSQL 语言重现 my_sum 函数：

```sql
forumdb=> CREATE OR REPLACE FUNCTION my_sum(x integer, y integer) RETURNS integer AS
$BODY$
DECLARE
  ret integer;
BEGIN
  ret := x + y;
  return ret;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
forumdb=> select my_sum(2,3);
```

| my_sum |
|--------|
| 5      |

(1 row)

前面的查询提供了与本章开头看到的查询相同的结果。现在，让我们更详细地检查一下：

1. 以下为函数头；在这里，你定义函数的名称、输入参数和返回值：

```sql
CREATE OR REPLACE FUNCTION my_sum(x integer, y integer) RETURNS integer AS
```

2. 以下为代码开始的标签。我们可以在 $$ 字符之间放置任何字符串；重要的是函数末尾要使用相同的标签：

```sql
$BODY$
```

3. 在以下部分，我们可以定义变量；重要的是每个声明或语句都以分号结束：

```sql
DECLARE
    ret integer;
```

4. 使用 BEGIN 语句，我们告诉 PostgreSQL 我们要开始编写逻辑：

```sql
BEGIN
```

> 注意：不要在 BEGIN 后写分号——这是不正确的，会导致语法错误。

5. 在 BEGIN 语句和 END 语句之间，我们可以放置自己的代码：

```sql
    ret := x + y;
    return ret;
```

6. END 指令表示我们的代码已结束：

```sql
END;
```

7. 该标签关闭第一个标签，最后，language 语句指定 PL/pgSQL，即函数所写的语言：

```sql
$BODY$
language 'plpgsql';
```

### 删除函数

要删除一个函数，我们必须执行 DROP FUNCTION 命令，后跟函数的名称及其参数。例如，要删除 my_sum 函数，我们必须执行：

```sql
forumdb=> DROP FUNCTION my_sum(integer, integer);
DROP FUNCTION
```

### 声明函数参数

在学习了如何编写简单的 PL/pgSQL 函数之后，让我们更详细地了解前一节中看到的各个方面。让我们从参数的声明开始。在接下来的两个示例中，我们将看到如何以两种不同的方式定义我们之前看到的 my_sum 函数。

第一个示例如下：

```sql
forumdb=> CREATE OR REPLACE FUNCTION my_sum(integer, integer) RETURNS integer AS
$BODY$
DECLARE
  x alias for $1;
  y alias for $2;
  ret integer;
BEGIN
  ret := x + y;
  return ret;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

第二个示例如下：

```sql
forumdb=> CREATE OR REPLACE FUNCTION my_sum(integer, integer) RETURNS integer AS
$BODY$
DECLARE
  ret integer;
BEGIN
  ret := $1 + $2;
  return ret;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

在第一个示例中，我们使用了 alias；alias 的语法通常如下：

```sql
newname ALIAS FOR oldname;
```

在我们的具体案例中，我们使用了位置变量 $1 作为 oldname 值。在第二个示例中，我们使用了位置方法，正如我们在 SQL 函数的情况下所做的那样。

### IN/OUT 参数

在前面的示例中，我们在函数定义的第五行使用了 RETURNS 子句；然而，达到相同目标还有另一种方法。在 PL/pgSQL 中，我们可以将所有参数定义为输入参数、输出参数或输入/输出参数。例如，假设我们写下以下内容：

```sql
forumdb=> CREATE OR REPLACE FUNCTION my_sum_3_params(IN x integer, IN y integer, OUT z integer) AS
$BODY$
```

这个结果看起来有点奇怪，因为结果不是标量值，而是一个记录，即自定义类型。为了使输出以列的形式分离，我们必须使用以下语法：

```sql
forumdb=> select * from my_sum_mul(2,3);
```

| w | z |
|---|---|
| 6 | 5 |

(1 row)

我们可以将函数的结果完全当作表的结果来使用，例如写下以下内容：

```sql
forumdb=> select * from my_sum_mul(2,3) where w=6;
```

| w | z |
|---|---|
| 6 | 5 |

(1 row)

我们可以按如下方式定义参数：

- IN：输入参数（如果省略，这是默认选项）
- OUT：输出参数
- INOUT：输入/输出参数

### 函数易变性类别

在 PostgreSQL 中，每个函数都可以定义为 VOLATILE、STABLE 或 IMMUTABLE。如果我们不指定任何内容，默认值是 VOLATILE。这三种可能定义之间的差异在官方文档中描述得很好（https://www.postgresql.org/docs/current/xfunc-volatility.html）：



一个 VOLATILE 函数可以做任何事情，包括修改数据库。它在相同参数的连续调用中可以返回不同的结果。优化器对这类函数的行为不做任何假设。使用易变函数的查询会在每次需要其值时在每一行重新计算该函数。如果一个函数被标记为 VOLATILE，那么如果我们使用相同的输入参数多次调用它，它可能会返回不同的结果。

一个 STABLE 函数不能修改数据库，并且保证在单个语句中所有行内给定相同参数时返回相同的结果。此类别允许优化器将函数的多次调用优化为一次调用。特别是，在索引扫描条件中使用包含此类函数的表达式是安全的。如果一个函数被标记为 STABLE，则在同一事务内给定相同参数时，该函数将返回相同的结果。

一个 IMMUTABLE 函数不能修改数据库，并且保证在给定相同参数时永远返回相同的结果。此类别允许优化器在查询使用常量参数调用函数时预先计算该函数。

在本章的后续页面中，我们将只关注易变函数的示例；然而，在这里我们将简要查看一个稳定函数的示例和一个不可变函数的示例：

- 1. 让我们从一个稳定函数开始——例如，now() 函数就是一个稳定函数。now() 函数返回我们在事务开始时拥有的当前日期和时间，如下所示：

```
forumdb=> begin ;
BEGIN

forumdb=* > select now();
    now
----------------------------
2023-03-17 13:25:25.37224+00
(1 row)

forumdb=* > select now();
        now
-----------------------------
2023-03-17 13:25:25.37224+00
(1 row)

forumdb=* > commit;
COMMIT

forumdb=> begin ;
BEGIN

forumdb=* > select now();
        now
-----------------------------
2023-03-17 13:27:02.012632+00
(1 row)

forumdb=* > commit ;
COMMIT
```

注意：在 PostgreSQL 16 中，当 psql 显示像 *> 这样的提示时，这意味着我们处于一个事务块中。

- 2. 现在，让我们看一个不可变函数——例如，lower(string_expression) 函数。lower 函数接受一个字符串并将其转换为小写格式。如我们所见，如果输入参数相同，则 lower 函数始终返回相同的结果，即使在不同的事务中执行：

```
forumdb=> begin;
BEGIN

forumdb=* > select now();
now
-----------------------------
2023-03-17 13:33:39.586388+00
(1 row)

forumdb=* > select lower('MICKY MOUSE');
    lower
----------------
micky mouse
(1 row)

forumdb=* > commit;
COMMIT

forumdb=> begin;
BEGIN

forumdb=* > select now();
       now
-----------------------------
2023-03-17 13:34:56.491773+00
(1 row)

forumdb=* > select lower('MICKY MOUSE');
    lower
----------------
micky mouse
(1 row)

forumdb=* > commit;
COMMIT
```

## 控制结构

PL/pgSQL 具有管理以下控制结构的能力：

- 条件语句
- 循环语句
- 异常处理语句

## 条件语句

PL/pgSQL 语言可以管理 IF 类型条件语句和 CASE 类型条件语句。

### IF 语句

在 PL/pgSQL 中，IF 语句的语法如下：

```
IF boolean-expression THEN
  statements
[ ELSIF boolean-expression THEN
  statements
[ ELSIF boolean-expression THEN
  statements
  ...
]
]
[ ELSE
  statements ]
END IF;
```

例如，假设我们想编写一个函数，当给定两个输入值 x 和 y 时，返回以下内容：

- *第一个参数大于第二个参数* 如果 x > y
- *第二个参数大于第一个参数* 如果 x < y
- *两个参数相等* 如果 x = y

我们必须编写以下函数：

```
forumdb=> CREATE OR REPLACE FUNCTION my_check(x integer default 0, y integer default 0) RETURNS text AS
$BODY$
BEGIN
  IF x > y THEN
  return 'first parameter is greater than second parameter';
ELSIF x < y THEN
return 'second parameter is greater than first parameter';
ELSE
return 'the 2 parameters are equals';
END IF;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

在这个例子中，我们看到了 IF 构造的最大形式：IF [...] THEN[...] ELSIF [...] ELSE[...] END IF；

然而，更短的形式也存在，如下所示：

- IF [...] THEN[...] ELSE[...] END IF;
- IF [...] THEN[...] END IF;

之前定义的函数提供的一些结果示例如下：

```
forumdb=> select my_check(1,2);
                my_check
---------------------------------------------
second parameter is higher than first parameter
(1 row)

forumdb=> select my_check(2,1);
                my_check
---------------------------------------------
first parameter is higher than second parameter
(1 row)

forumdb=> select my_check(1,1);
                my_check
---------------------------------------------
the 2 parameters are equals
(1 row)
```

### CASE 语句

在 PL/pgSQL 中，也可以使用 CASE 语句。CASE 语句可以有以下两种语法。

以下是简单 CASE 语句：

```
CASE search-expression
  WHEN expression [, expression [ ... ]] THEN
  statements
  [ WHEN expression [, expression [ ... ]] THEN
  statements
  ... ]
  [ ELSE
  statements ]
END CASE;
```

以下是搜索 CASE 语句：

```
CASE
  WHEN boolean-expression THEN
  statements
  [ WHEN boolean-expression THEN
  statements
  ... ]
  [ ELSE
  statements ]
END CASE;
```

现在，我们将执行以下操作：

- 我们将使用第一个，即简单 CASE 语法，如果我们需要从一组值中做出选择。
- 我们将使用第二个，当我们需要从一系列值中选择时。

让我们从第一个语法开始：

```
forumdb=> CREATE OR REPLACE FUNCTION my_check_value(x integer default 0)
RETURNS text AS
$BODY$
BEGIN
CASE x
WHEN 1 THEN return 'value = 1';
WHEN 2 THEN return 'value = 2';
ELSE return 'value >= 3 ';
END CASE;
END;
$BODY$
language 'plpgsql';
CREATE FUNCTION
```

前面的 my_check_value 函数返回以下内容：

- value = 1 如果 x = 1
- value = 2 如果 x = 2
- value >= 3 如果 x >= 3

我们可以在这里看到这是正确的：

```
forumdb=> select my_check_value(1);
my_check_value
----------------
value = 1
(1 row)

forumdb=> select my_check_value(2);
my_check_value
----------------
value = 2
(1 row)

forumdb=> select my_check_value(3);
my_check_value
----------------
value >= 3
(1 row)
```

现在，让我们看一个搜索 CASE 语法的示例：

```
forumdb=> CREATE OR REPLACE FUNCTION my_check_case(x integer default 0, y integer default 0) RETURNS text AS
$BODY$
BEGIN
    CASE
        WHEN x > y THEN return 'first parameter is higher than second parameter';
        WHEN x < y THEN return 'second parameter is higher than first parameter';
        ELSE return 'the 2 parameters are equals';
    END CASE;
END;
$BODY$
    language 'plpgsql';
CREATE FUNCTION
```

my_check_case 函数返回的数据与我们之前编写的 my_check 函数相同：

```
forumdb=> select my_check_case(2,1);
                my_check_case
--------------------------------------------------
 first parameter is higher than second parameter
(1 row)

forumdb=> select my_check_case(1,2);
                my_check_case
--------------------------------------------------
 second parameter is higher than first parameter
(1 row)

forumdb=> select my_check_case(1,1);
                my_check_case
-----------------------------
 the 2 parameters are equals
(1 row)

forumdb=> select my_check_case();
                my_check_case
-----------------------------
```

```
for rw in select * from posts where pk=p_id loop
    ret.id := rw.pk;
    ret.title := rw.title;
    ret.record_data := hstore(ARRAY['title',rw.title,'Title and Content',format('%s %s',rw.title,rw.content)]);
    return next ret;
    end loop;
    return;
END;
$$
language 'plpgsql';
CREATE FUNCTION
```

正如我们所见，这几行 PL/pgSQL 代码中集中了许多内容：

- 1. `rw posts%ROWTYPE`：通过此语句，rw 变量被定义为 posts 表的一行容器的容器。
- 2. `for rw in select * from posts where pk=p_id loop`：通过此语句，我们在选择结果中循环，每次将 select 命令返回的值分配给 rw 变量。接下来的三个步骤将值分配给 ret 变量。
- 3. `return next ret;`：此语句返回 ret 变量的值并转到 for 循环的下一条记录。
- 4. `end loop;`：此语句告诉 PostgreSQL for 循环在此结束。
- 5. `return;`：这是函数的返回指令。

需要记住的重要一点是，PL/pgSQL 语言位于 PostgreSQL 事务系统内部。这意味着函数是原子执行的，并且函数不是在执行 RETURN NEXT 命令时返回结果，而是在执行放置在函数末尾的 RETURN 命令时返回结果。这可能意味着对于非常大的数据集，PL/pgSQL 函数在返回结果之前可能需要很长时间。

## 记录类型

在我们之前使用过的一个示例中，我们引入了 %ROWTYPE 数据类型。在 PL/pgSQL 语言中，可以将此概念推广。有一种名为 record 的数据类型，它推广了 %ROWTYPE 的概念。





例如，我们可以按如下方式重写 my_first_fun：

```
forumdb=> CREATE OR REPLACE FUNCTION my_second_fun (p_id integer) returns setof my_ret_type as
$$
DECLARE
    rw record; -- declare a record variable
    ret my_ret_type;
BEGIN
    for rw in select * from posts where pk=p_id loop
    ret.id := rw.pk;
    ret.title := rw.title;
    ret.record_data := hstore(ARRAY['title',rw.title
        ,'Title and Content',format('%s %s',rw.title,rw.content)]);
        return next ret;
    end loop;
    return;
END;
$$
language 'plpgsql';
CREATE FUNCTION
```

my_first_fun 和 my_second_fun 之间唯一的差别在于以下定义：

```
rw record; -- declare a record variable
```

这次，rw 变量被定义为 record 数据类型。这意味着 rw 变量是一个可以与任何表的任何记录关联的对象。两个函数 my_first_fun 和 my_second_fun 的结果是相同的：

```
forumdb=> \x
Expanded display is on.
forumdb=> select * from my_first_fun(5);
-[ RECORD 1 ]-----------------
id          | 5
title       | Indexing PostgreSQL
record_data | "title"=>"Indexing PostgreSQL", "Title and Content"=>"Indexing PostgreSQL Btree in PostgreSQL is...."
```

```
$$ DECLARE
    ret real;
BEGIN
    ret := x / y;
    return ret;
EXCEPTION
    WHEN division_by_zero THEN
        RAISE INFO 'DIVISION BY ZERO';
        RAISE INFO 'Error % %', SQLSTATE, SQLERRM;
        RETURN 0;
END;
$$ language 'plpgsql' ;
CREATE FUNCTION
```

SQLSTATE 和 SQLERRM 变量包含与生成的错误相关的状态和消息。现在，如果我们执行第二个函数，我们不再从 PostgreSQL 得到错误：

```
forumdb=> select my_second_except(4,0);
INFO:  DIVISION BY ZERO
INFO:  Error 22012 division by zero
my_second_except
-----------------
                0
(1 row)
```

PostgreSQL 可以处理的错误列表可在 https://www.postgresql.org/docs/current/errcodes-appendix.html 获取。

## Security definer

此选项允许用户像其所有者一样调用函数。在所有我们想要向普通用户无法访问的数据显示数据的情况下，它可能很有用。

例如，在 PostgreSQL 中，有一个名为 pg_stat_activity 的系统视图，允许我们查看 PostgreSQL 当前正在做什么。

作为用户 forum，让我们执行此语句：

```
postgres@learn_postgresql:~$ psql -U forum forumdb
```

```
forumdb=> 

forumdb=> select pid,query from pg_stat_activity ;
 pid |            query
-------+-------------------------------
 74 | <insufficient privilege>
 75 | <insufficient privilege>
 217 | select pid,query from pg_stat_activity ;
 [...]
```

如上所示，有一些 <insufficient privilege> 结果。以下是解决此问题的步骤：

- 让我们以用户 postgres 连接到数据库：

```
postgres@learn_postgresql:~$ psql forumdb
forumdb=#
```

- 现在让我们执行此处编写的函数 my_stat_activity()：

```
forumdb=# create function forum.my_stat_activity()
returns table (pid integer,query text)
as $$
    select pid, query  from pg_stat_activity;
$$ language 'sql'
security definer;
```

- 让我们将对函数 my_stat_activity 的执行权限授予 forum 用户。我们将在第 10 章《授予和撤销权限》中看到此功能：

```
forumdb=# grant execute on function forum.my_stat_activity TO forum;
```

- 让我们再次以用户 forum 连接到数据库：

```
postgres@learn_postgresql:~$ psql -U forum forumdb
forumdb=>
```

- 现在让我们执行下面编写的查询：

```
forumdb=> select * from my_stat_activity();
 pid |            query
-------+-------------------------------
```

```
74 |
75 |
271 | select * from my_stat_activity();
[...]
```

我们不再有之前遇到的问题。这是因为 security definer 允许 forum.my_stat_activity() 函数以创建它的用户的权限执行，在本例中，创建它的用户是 postgres 用户。

## Summary

在本章中，我们介绍了服务器端编程的世界。该主题如此庞大，以至于有专门的书籍只致力于此。我们试图让您更好地理解服务器端编程的主要概念。我们谈到了 PostgreSQL 管理的主要数据类型，然后我们看到了如何使用复合数据类型创建新的数据类型。我们还提到了 SQL 函数和多态函数，最后，我们提供了有关 PL/pgSQL 语言的一些信息。

在下一章中，我们将使用这些概念来介绍 PostgreSQL 中的事件管理。我们将通过使用触发器及其相关函数来讨论事件管理。

## Verify your knowledge

- 是否可以扩展 是否可以扩展 postgresql 的功能和数据类型？
  可以，我们可以在数据类型方面和函数方面扩展 PostgreSQL。
  有关更多详细信息，请参阅 The concept of extensibility 部分。
- PostgreSQL 是否仅支持关系数据库？
  不，PostgreSQL 也支持 NoSQL 数据库。
  有关更多详细信息，请参阅 The NoSql data type 部分。
- PostgreSQL 是否支持 SQL 函数？
  是的，我们可以编写任何类型的 SQL 函数。
  有关更多详细信息，请参阅 SQL functions 部分。

## 8
