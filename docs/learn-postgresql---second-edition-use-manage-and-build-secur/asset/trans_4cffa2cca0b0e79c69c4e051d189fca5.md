

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

