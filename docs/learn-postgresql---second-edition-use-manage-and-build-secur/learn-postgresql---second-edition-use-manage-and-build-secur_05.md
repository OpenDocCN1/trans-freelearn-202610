# 9

# Partitioning

在上一章中，我们讨论了规则和触发器。在本章中，我们将讨论分区。分区是一种技术，允许我们将巨大的表拆分为较小的表以使查询更高效。在本章中，我们将了解如何对数据进行分区，并且在某些情况下，如何使用上一章中看到的规则和触发器来实现分区。我们将首先介绍分区的概念，然后了解 PostgreSQL 实现分区的方法。

本章将涵盖以下主题：

- Basic concepts
- Partitioning using table inheritance
- Declarative partitioning

## Technical requirements

本章示例可以在本书 GitHub 仓库中找到的 chapter_09 Docker 镜像上运行：https://github.com/PacktPublishing/Learn-PostgreSQL-Second-Edition。有关本书 Docker 镜像的安装和使用说明，请参阅第 1 章，Introduction to PostgreSQL。

## Basic concepts

首先，让我们尝试理解为什么我们必须对数据进行分区。我们应该首先说，所有数据库的一个共同常量是它们的大小总是增长。因此，数据库在增长几个月后，可能达到 gigabytes、terabytes 甚至 petabytes 的大小。

我们还必须始终牢记，并非所有表都以相同的速率或达到相同的水平增长；有些表比其他表更大，有些索引也比其他索引更大。

我们还需要知道，我们服务器的 RAM 中有一部分由所有 PostgreSQL 进程共享，用于管理表中的数据。服务器 RAM 的这一部分称为 `shared_buffers`。

PostgreSQL 的工作方式如下：

1. 数据从硬盘中取出。
2. 数据放置在 shared buffers 中。
3. 数据在 shared buffers 中处理。
4. 数据下载到磁盘。

通常，在仅用于 PostgreSQL 的专用服务器上，`shared_buffers` 的大小约为服务器总 RAM 的三分之一或四分之一。设置一些 PostgreSQL 配置参数的有用链接（包括 `shared_buffers` 的推荐大小）是 https://pg tune.leopard.in.ua。

当表的增长相对于 `shared_buffers` 大小过度时，性能可能会下降。在这种情况下，对数据进行分区可以帮助我们。分区数据意味着以对客户端程序透明的方式将非常大的表拆分为较小的表。客户端程序会认为服务器仍然只有一个表，但拥有较小的表也意味着拥有较小的索引，这些索引更有可能保留在内存中，这反过来又提高了数据性能；此外，较小的表意味着 vacuum 进程在较小的表上工作，这最大限度地减少了 vacuum 进程的执行时间。最后，在运行 vacuum full 时，表使用的磁盘空间会翻倍，因此拥有许多小表而不是一个大表会显著减少由此引起的影响。数据分区可以通过两种方式完成：

- 使用表继承（PostgreSQL < 10 的唯一可能方式）
- 使用声明式分区（从版本 10 开始的最佳方式）

确定何时建议对数据进行分区后，让我们看看可能的表分区类型。PostgreSQL 支持三种声明式分区类型：

- Range partitioning
- List partitioning
- Hash partitioning

我们现在将详细介绍这三种方法。





开始之前，请记住启动名为 chapter9 的 Docker 容器，如下所示：

```
$ bash run-pg-docker.sh chapter_09
postgres@learn_postgresql:~$ psql -U forum forumdb
```

## 范围分区

范围分区是将表划分为“区间”。区间必须不重叠，范围是通过使用一个字段或一组字段来定义的。有关更多信息，请参阅 https://www.postgresql.org/docs/current/ddl-partitioning.html。

让我们来看一个范围分区的定义示例。假设我们有这个表：

| field date | field_value |
|------------|-------------|
| 2023-03-01 | 1 |
| 2023-03-02 | 10 |
| 2023-04-01 | 12 |
| 2023-04-15 | 1 |

表 9.1：范围分区前的表

现在假设我们想将此表拆分为两个表。第一个表（TABLE A）将包含所有 field_date 值介于 2023-03-01 和 2023-03-31 之间的记录，第二个表（TABLE B）将包含所有 field_date 值介于 2023-04-01 和 2023-04-30 之间的记录。因此，我们的目标是拥有如下两个表：

| field date | field_value |
|------------|-------------|
| 2023-03-01 | 1 |
| 2023-03-02 | 10 |

表 9.2：表 A

| field date | field_value |
|------------|-------------|
| 2023-04-01 | 12 |
| 2023-04-15 | 1 |

表 9.3：表 B

我们看到的是按范围分区的一个示例。当我们拥有可以按时间范围划分的大表时，这很有用，例如，营业额、审计表或日志表。

## 列表分区

在列表分区中，表将使用值列表进行分区。

让我们来看一个列表分区的定义示例。假设我们有这个表：

| field_state | field_city |
|---|---|
| United States | Washington |
| United States | San Francisco |
| Italy | Rome |
| Japan | Tokyo |

*表 9.4：列表分区前的表*

现在假设我们想将此表拆分为 n 个表，每个州一个表。第一个表（TABLE A）将包含所有 field_state 值等于 United States 的记录，第二个表（TABLE B）将包含所有 field_state 值等于 Italy 的记录，第三个表（TABLE C）将包含 field_state 值等于 Japan 的记录。因此，我们的目标是拥有如下三个表：

| field_state | field_city |
|---|---|
| United States | Washington |
| United States | San Francisco |

*表 9.5：表 A*

| field_state | field_city |
|---|---|
| Italy | Rome |

*表 9.6：表 B*

| field_state | field_city |
|---|---|
| Japan | Tokyo |

*表 9.7：表 C*

这是一个按列表分区的示例。当我们拥有可以按单个字段划分的大表时，这很有用，例如，电话簿中的城市或州字段，或客户列表。

## 哈希分区

使用哈希分区，表将使用哈希值进行分区，以将数据拆分到不同的表中。

让我们来看一个哈希分区的示例。假设我们有这个表：

| field date | field_value |
|---|---|
| 2023-03-01 | 1 |
| 2023-03-02 | 1 |
| 2023-04-01 | 2 |
| 2023-04-15 | 2 |

表 9.8：哈希分区前的表

现在假设我们有一个哈希函数，它将日期转换为哈希值；例如，让我们考虑 del mod 运算符（%）：

- hash(1) = 1
- hash(1) = 1
- hash(2) = 0
- hash(2) = 0

因此，分区过程后，我们将有两个表：

| field date | field_value |
|---|---|
| 2023-03-01 | 1 |
| 2023-03-02 | 1 |

表 9.9：表 A

| field date | field_value |
|---|---|
| 2023-04-01 | 2 |
| 2023-04-15 | 2 |

表 9.10：表 B

这是一个按哈希分区的示例。

在下面的各节中，我们将看到 PostgreSQL 如何实现列表、范围和哈希分区，但在那之前，让我们花点时间谈谈表继承。

有关分区的信息，请参阅 https://www.postgresql.org/docs/current/ddl-partitioning.html。

## 表继承

我们必须查看的另一个主题是表的继承。PostgreSQL 采用了数据库到对象的继承概念。该概念非常简单，可以总结如下：假设我们有两个表，TABLE A 和 TABLE B。如果我们将 TABLE A 定义为父表，将 TABLE B 定义为子表，这意味着 TABLE B 中的所有记录都可以通过 TABLE A 访问。

让我们现在尝试举例说明我们刚刚描述的内容：

- 1. 让我们定义两个表。

第一个表，父表，定义如下：

```
forumdb=> create table table_a (
 pk integer not null primary key,
 tag text,
 parent integer);
CREATE TABLE
```

第二个表，子表，定义如下：

```
forumdb=> create table table_b () inherits (table_a);
CREATE TABLE

forumdb=> alter table table_b add constraint table_b_pk primary key(pk);
ALTER TABLE
```

- 2. 子表继承父表的所有字段。父表如下所示：

```
forumdb=> \d table_a;
               Table "forum.table_a"
 Column | Type | Collation | Nullable | Default
--------+----------+------------+----------+---------
 pk     | integer |            | not null |
```

```
Column | Type | Collation | Nullable | Default
----------+-----------------+---------------+-----------+----------
pk | integer | | not null |
tag | text | | |
parent | integer | | |
Indexes:
  "table_b_pk" PRIMARY KEY, btree (pk)
Inherits: table_a
```

这里，我们可以看到 table_b 是 table_a 的子表。

4. 现在让我们看看如果我们插入、修改或删除数据，这两个表会如何表现。例如，让我们按如下方式进行一些插入：

```
forumdb=> insert into table_a (pk,tag,parent) values (1,'Operating Systems',0);
INSERT 0 1

forumdb=> insert into table_b (pk,tag,parent) values (2,'Linux',0);
INSERT 0 1
```

5. 让我们看看如果我们执行 select 命令，我们的数据会如何反应：

```
forumdb=> select * from table_b ;
pk | tag | parent
----+-------+--------
  2 | Linux | 0
(1 row)
```

我们可以看到 table_b 有一条记录。

6. 现在我们执行以下命令：

```
forumdb=> select * from table_a ;
pk | tag | parent
```

```
----+---------------+-------
  1 | Operating Systems |     0
  2 | Linux           |     0
(2 rows)
```

似乎 table_a 有两条记录。这是因为该表继承了其他表的属性。如果我们在父表上执行 SELECT 命令，我们将看到属于父表的所有记录以及属于子表的所有记录。

7. 如果我们只想查看属于 table_a 的记录，我们必须使用 ONLY 子句，如下所示：

```
forumdb=> select * from only table_a ;
pk |      tag      | parent
----+---------------+-------
  1 | Operating Systems |     0
(1 row)
```

8. 让我们看看如果我们 UPDATE 一些记录会发生什么，例如，如果我们执行以下操作：

```
forumdb=> update table_a set tag='BSD Unix' where pk=2;
UPDATE 1
```

我们在 table_a 上执行了更新操作，但此更新通过表的可继承性在物理上在 table_b 上完成，如下所示：

```
forumdb=> select * from table_b;
pk |  tag   | parent
----+------------+-------
  2 | BSD Unix |     0
(1 row)
```

9. 如果我们使用如下 delete 语句，也会发生同样的情况：

```
forumdb=> delete from table_a where pk=2;
DELETE 1
```

这里，同样，对 table_a 执行的删除操作对其在 table_b 上的效果；如下所示，table_a 将有这些记录：

```
forumdb=> select * from table_a;
pk |      tag      | parent
----+---------------+--------
  1 | Operating Systems |     0
(1 row)
```

而 table_b 现在将没有记录：

```
forumdb=> select * from table_b;
pk | tag | parent
----+-----+--------
(0 rows)
```

在 PostgreSQL 中，继承会将对父表执行的操作传播到子表。

## 删除表

为了结束继承的主题，我们需要解决如何删除表的问题。如果我们想删除一个子表，例如，要删除 table_b，我们必须运行以下语句：

```
forumdb=> drop table table_b;
DROP TABLE
```

如果我们想删除父表及其所有链接的子表，我们必须运行以下语句：

```
forumdb=> drop table table_a cascade;
```

虽然继承已被使用并且仍然可以用来实现表分区，但从版本 10 开始，声明式分区已成为首选方法。我们在下一节中介绍声明式分区。

## 探索声明式分区

在本节中，我们将讨论声明式分区。它自 PostgreSQL 版本 10 起可用，但在较新版本中的性能有所提高。我们现在将看一个按范围分区的示例和一个按列表分区的示例。





# 列表分区

在声明式分区的第一个示例中，我们将使用之前介绍继承分区时所用的同一个示例。我们会看到，使用声明式分区方法后，一切变得简单得多：

+   1. 现在让我们创建父表：

```sql
forumdb=> CREATE TABLE part_tags (
  pk SERIAL NOT NULL ,

  level INTEGER NOT NULL DEFAULT 0,
  tag VARCHAR (255) NOT NULL,
  primary key (pk,level)
)
PARTITION BY LIST (level);
```

从前面的示例可以看出，我们必须定义要应用的分区类型。在本例中，它是 LIST PARTITIONING，即列表分区。另一个需要注意的重要事项是，用于分区数据的字段必须是主键的一部分。

+   2. 接下来，让我们定义子表：

```sql
forumdb=> CREATE TABLE part_tags_level_0 PARTITION OF part_tags FOR VALUES IN (0);
CREATE TABLE part_tags_level_1 PARTITION OF part_tags FOR VALUES IN (1);
CREATE TABLE part_tags_level_2 PARTITION OF part_tags FOR VALUES IN (2);
CREATE TABLE part_tags_level_3 PARTITION OF part_tags FOR VALUES IN (3);
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
```

通过这些 SQL 语句，我们定义了所有 level 值等于 0 的记录将存储在 part_tags_level_0 表中，所有 level 值等于 1 的记录将存储在 part_tags_level_1 表中，以此类推。

3. 现在，让我们为父表定义索引。这些索引将自动传播到子表。我们可以使用以下简单语句完成此操作：

```
forumdb=> CREATE INDEX on part_tags (tag);
CREATE INDEX
```

4. 如图所示，我们的分区过程已完成。

对于父表，我们有以下内容：

```
forumdb=> \d part_tags;

Partitioned table "forum.part_tags"

Column | Type | Collation | Nullable | Default

----------------+---------------+-----------+----------+------------------------

pk | integer | | not null | nextval('part_tags_pk_seq'::regclass)
level | integer | | not null | 0
tag | character varying(255) | | not null |

Partition key: LIST (level)
Indexes:
"part_tags_pkey" PRIMARY KEY, btree (pk, level)
"part_tags_tag_idx" btree (tag)
Number of partitions: 4 (Use \d+ to list them.)
```

对于子表，我们有以下内容：

```
forumdb=> \d part_tags_level_0;

Table "forum.part_tags_level_0"

Column | Type | Collation | Nullable | Default

----------------+---------------+-----------+----------+------------------------

pk | integer | | not null | nextval('part_tags_pk_seq'::regclass)
level | integer | | not null | 0
tag | character varying(255) | | not null |
```

```
Partition of: part_tags FOR VALUES IN (0)
Indexes:
  "part_tags_level_0_pkey" PRIMARY KEY, btree (pk, level)
  "part_tags_level_0_tag_idx" btree (tag)
```

5. 现在让我们执行一些 INSERT 操作：

```
forumdb=> insert into part_tags (tag,level) values ('Operating System',0);
INSERT 0 1
forumdb=> insert into part_tags (tag,level) values ('Linux',1);
INSERT 0 1
forumdb=> insert into part_tags (tag,level) values ('BSD Unix',1);
INSERT 0 1
forumdb=> insert into part_tags (tag,level) values ('DOS',1);
INSERT 0 1
forumdb=> insert into part_tags (tag,level) values ('Windows',2);
INSERT 0 1
```

6. 最后，让我们检查一切是否正常：

```
forumdb=> select * from part_tags;
pk | level |      tag
----+-------+-----------------
  1 |     0 | Operating System
  2 |     1 | Linux
  3 |     1 | BSD Unix
  4 |     1 | DOS
  5 |     2 | Windows
(5 rows)

forumdb=> select * from part_tags_level_0;
pk | level |      tag
----+-------+-----------------
  1 |     0 | Operating System
```

```
(1 row)

forumdb=> select * from part_tags_level_1;
pk | level |   tag
----+-------+--------
  2 |     1 | Linux
  3 |     1 | BSD Unix
  4 |     1 | DOS
(3 rows)
forumdb=> select * from part_tags_level_2;
pk | level |   tag
----+-------+--------
  5 |     2 | Windows
(1 row)
```

因此，我们已成功使用列表创建了分区。

# 范围分区

在看到如何以非常简单的方式按列表分区之后，让我们看看如何按范围分区：

+   1. 如前所述，让我们 DROP 现有的 part_tags 表及其子表：

```
forumdb=> DROP TABLE IF EXISTS part_tags cascade;
DROP TABLE
```

+   2. 假设我们想要一个与之前完全相同的表，但现在我们希望 part_tags 表具有 ins_date 字段，我们将在其中存储标签添加的日期。我们想要按 ins_date 字段的范围分区，以便将 2023 年 1 月、2023 年 2 月、2023 年 3 月和 2023 年 4 月输入的记录放入不同的表中。这里有所有使此操作成为可能的语句；它们与我们在上一节中看到的语句非常相似：

```
forumdb=> CREATE TABLE part_tags (
    pk serial NOT NULL,
    ins_date date not null default now()::date,
    tag VARCHAR (255) NOT NULL,
```

```
level INTEGER NOT NULL DEFAULT 0,
    primary key (pk,ins_date)
)
PARTITION BY RANGE (ins_date);
CREATE TABLE

forumdb=> CREATE TABLE part_tags_date_01_2023 PARTITION OF part_tags
FOR VALUES FROM ('2023-01-01') TO ('2023-01-31');
CREATE TABLE

forumdb=> CREATE TABLE part_tags_date_02_2023 PARTITION OF part_tags
FOR VALUES FROM ('2023-02-01') TO ('2023-02-28');
CREATE TABLE

forumdb=> CREATE TABLE part_tags_date_03_2023 PARTITION OF part_tags
FOR VALUES FROM ('2023-03-01') TO ('2023-03-31');
CREATE TABLE

forumdb=> CREATE TABLE part_tags_date_04_2023 PARTITION OF part_tags
FOR VALUES FROM ('2023-04-01') TO ('2023-04-30');
CREATE TABLE

forumdb=> CREATE INDEX on part_tags(tag);
CREATE INDEX
```

正如我们所见，唯一的不同之处只有 PARTITION BY RANGE 和 FOR VALUES FROM .. TO ...

+   3. 在本示例中，与之前的列表分区示例一样，我们在没有复杂性的情况下获得了父表和所有子表，并且正如以下片段所示，CREATE INDEX 语句已自动传播到子表：

```
forumdb=> \d part_tags;
            Partitioned table "forum.part_tags"
    Column |     Type     | Collation | Nullable | Default
```

```
----------------+----------------------+----------+----------
pk             | integer              |          | not null |
nextval('part_tags_pk_seq'::regclass)
ins_date       | date                 |          | not null |
now()::date
tag            | character varying(255) |        | not null |

level          | integer              |          | not null | 0
Partition key: RANGE (ins_date)
Indexes:
    "part_tags_pkey" PRIMARY KEY, btree (pk, ins_date)
    "part_tags_tag_idx" btree (tag)
Number of partitions: 4 (Use \\d+ to list them.)

forumdb=> \\d part_tags_date_01_2023;
                         Table "forum.part_tags_date_01_2023"
 Column   |          Type          | Collation | Nullable |
Default
----------+------------------------+-----------+----------

----------------+----------------------+----------+----------
pk       | integer                 |          | not null |
nextval('part_tags_pk_seq'::regclass)
ins_date | date                    |          | not null |
now()::date
tag      | character varying(255)  |          | not null |

level    | integer                 |          | not null | 0
Partition of: part_tags FOR VALUES FROM ('2023-01-01') TO ('2023-01-31')
Indexes:
    "part_tags_date_01_2023_pkey" PRIMARY KEY, btree (pk, ins_date)
    "part_tags_date_01_2023_tag_idx" btree (tag)
```

4. 如同之前所做，让我们做一些 INSERT 操作：

```
forumdb=> insert into part_tags (tag,ins_date,level) values ('Operating Systems','2023-01-01',0);
INSERT 0 1
forumdb=> insert into part_tags (tag,ins_date,level) values ('Linux','2023-02-01',1);
INSERT 0 1
forumdb=> insert into part_tags (tag,ins_date,level) values ('BSD Unix','2023-03-01',1);
INSERT 0 1
forumdb=> insert into part_tags (tag,ins_date,level) values ('Rocky Linux Distro','2023-04-01',2);
INSERT 0 1
```

5. 现在让我们检查一切是否正常：

```
forumdb=> select * from part_tags;
pk | ins_date |      tag      | level
----+----------+----------------+-------
  1 | 2023-01-01 | Operating Systems |   0
  2 | 2023-02-01 | Linux          |   1
  3 | 2023-03-01 | BSD Unix       |   1
  4 | 2023-04-01 | Rocky Linux Distro | 2
(4 rows)

forumdb=> select * from part_tags_date_01_2023;
pk | ins_date |      tag      | level
----+----------+----------------+-------
  1 | 2023-01-01 | Operating Systems |   0
(1 row)

forumdb=> select * from part_tags_date_02_2023;
pk | ins_date | tag | level
----+----------+-----+-------
```





```
2 | 2023-02-01 | Linux |
(1 row)

forumdb=> select * from part_tags_date_03_2023;
pk | ins_date | tag | level

----+------------+------------+------
 3 | 2023-03-01 | BSD Unix | 1
(1 row)
forumdb=> select * from part_tags_date_04_2023;
pk | ins_date | tag | level

----+------------+--------------------+------
 4 | 2023-04-01 | Rocky Linux Distro | 2
(1 row)
```

正如我们所见，所有数据都已正确分区。

## 分区维护

在前两节中，我们看到了声明式分区是什么，以及在从零开始工作时如何创建分区表。在本节中，我们将研究如何在分区表已存在的情况下附加或分离分区。我们将查看如何执行以下操作：

- 附加新分区
- 分离现有分区
- 将现有表附加到父表

## 附加新分区

如果我们想向父表附加一个新分区，我们必须执行以下操作：

```
forumdb=> CREATE TABLE part_tags_date_05_2023 PARTITION OF part_tags FOR
VALUES FROM ('2023-05-01') TO ('2023-05-30');
CREATE TABLE
```

正如我们在此处所见，一个名为 part_tags_date_05_2023 的新分区已添加到 part_tags 父表：

```
forumdb=> \d+ part_tags;
              Partitioned table "forum.part_tags"
```

```
Column | Type | [...] | Description
-----------------+---------------+----------------
pk | integer | [...] |
ins_date | date | [...] |
tag | character varying(255) | [...] |
level | integer | [...] |
Partition key: RANGE (ins_date)
Indexes:
  "part_tags_pkey" PRIMARY KEY, btree (pk, ins_date)
  "part_tags_tag_idx" btree (tag)
Partitions: part_tags_date_01_2023 FOR VALUES FROM ('2023-01-01') TO ('2023-01-31'),
  part_tags_date_02_2023 FOR VALUES FROM ('2023-02-01') TO ('2023-02-28'),
  part_tags_date_03_2023 FOR VALUES FROM ('2023-03-01') TO ('2023-03-31'),
  part_tags_date_04_2023 FOR VALUES FROM ('2023-04-01') TO ('2023-04-30'),
  part_tags_date_05_2023 FOR VALUES FROM ('2023-05-01') TO ('2023-05-30')
```

## 分离现有分区

如果我们想从父表分离一个现有分区，我们必须执行以下操作：

```
forumdb=> ALTER TABLE part_tags DETACH PARTITION part_tags_date_05_2023 ;
ALTER TABLE
```

正如我们在此处所见，名为 part_tags_date_05_2023 的分区已从 part_tags 父表分离：

```
forumdb=> \d+ part_tags;
Partitioned table "forum.part_tags"
Column | Type | [...] | Description
```

```
-----------------+---------------+----------
pk      | integer       | [...] |
ins_date | date          | [...] |
tag     | character varying(255) | [...] |
level   | integer       | [...] |

Partition key: RANGE (ins_date)
Indexes:
    "part_tags_pkey" PRIMARY KEY, btree (pk, ins_date)
    "part_tags_tag_idx" btree (tag)
Partitions: part_tags_date_01_2023 FOR VALUES FROM ('2023-01-01') TO ('2023-01-31'),
            part_tags_date_02_2023 FOR VALUES FROM ('2023-02-01') TO ('2023-02-28'),
            part_tags_date_03_2023 FOR VALUES FROM ('2023-03-01') TO ('2023-03-31'),
            part_tags_date_04_2023 FOR VALUES FROM ('2023-04-01') TO ('2023-04-30')
```

## 将现有表附加到父表

为了练习这一点，我们需要在数据库中有一个名为 part_tags_already_exists 的表，并且其中包含所有标签条目日期早于 2022-12-31 的数据。如果你使用的是 Docker 镜像，你可以在 forumdb 数据库中找到它。否则，请确保使用以下结构创建表：

```
forumdb=> \d part_tags_already_exists
                 Table "forum.part_tags_already_exists"
 Column |      Type      | Collation | Nullable | Default
--------+----------------+-----------+----------+--------
pk      | integer        |           | not null |
ins_date | date           |           | not null |
```

为此，让我们执行以下语句：

```
forumdb=> CREATE TABLE part_tags_default PARTITION OF part_tags default;
CREATE TABLE
```

现在让我们重复之前的条目：

```
forumdb=> insert into part_tags (tag,ins_date,level) values ('Ubuntu Linux','2023-05-01',2);
INSERT 0 1
```

此时，数据已插入到默认分区中，并且可以从 part_tags 父表看到，正如我们在此处所见：

```
forumdb=> select * from part_tags;
pk | ins_date |      tag      | level
----+----------+----------------+-------
 1 | 2023-01-01 | Operating Systems |   0
 2 | 2023-02-01 | Linux          |   1
 3 | 2023-03-01 | BSD Unix       |   1
 4 | 2023-04-01 | Rocky Linux Distro |   2
 6 | 2023-05-01 | Ubuntu Linux   |   2
(5 rows)

forumdb=> select * from part_tags_default ;
pk | ins_date |      tag      | level
----+----------+----------------+-------
 6 | 2023-05-01 | Ubuntu Linux   |   2
(1 row)
```

## 分区和表空间

现在假设我们想将第 2 章中看到的表空间与我们刚刚看到的分区程序一起使用。使用这种技术，我们将能够将子表放置在不同的表空间上，因此放置在不同的目录中，这些目录可以挂载在不同的卷上。

这种工作方式可以提高读写性能。在下面的示例中，我们将仅限于在本地目录上创建两个表空间。但是，使用 mount 命令将这两个目录映射到不同卷上并不困难。如果你使用的是本章提供的 Docker 镜像，我们将使用的两个目录已经可用。

如果你没有使用 Docker 镜像，你首先需要创建两个目录，`/data/tablespaces/ts_a` 和 `/data/tablespaces/ts_b`，其中 postgres 系统用户能够读写数据。

现在让我们以 postgres 用户身份连接到 forumdb 数据库并创建两个名为 ts_a 和 ts_b 的表空间：

```
postgres@learn_postgresql:~$ psql -U postgres forumdb

forumdb=# create tablespace ts_a location '/data/tablespaces/ts_a';
CREATE TABLESPACE
forumdb=# create tablespace ts_b location '/data/tablespaces/ts_b';
CREATE TABLESPACE
```

让我们将所有权分配给 postgres 用户：

```
forumdb=# alter tablespace ts_a owner to forum ;
ALTER TABLESPACE
forumdb=#  alter tablespace ts_b owner to forum ;
ALTER TABLESPACE
```

现在让我们以 forum 用户身份重新连接到 forumdb 数据库：

```
forumdb=# \q
postgres@learn_postgresql:~$ psql -U forum forumdb
```

与前一种情况一样，让我们重新创建父表：

```
forumdb=> CREATE TABLE tablespace_part_tags (
    pk serial NOT NULL,
    ins_date date not null default now()::date,
    tag VARCHAR (255) NOT NULL,
    level INTEGER NOT NULL DEFAULT 0,
```

```
primary key (pk,ins_date)
)
PARTITION BY RANGE (ins_date);
CREATE TABLE
```

现在让我们创建两个子表和一个默认表。第一个子表将在表空间 ts_a 上创建，第二个在表空间 ts_b 上创建：

```
forumdb=> CREATE TABLE tablespace_part_tags_date_2022 PARTITION OF tablespace_part_tags FOR VALUES FROM ('2021-01-01') TO ('2022-12-31') TABLESPACE ts_a;
CREATE TABLE
forumdb=> CREATE TABLE tablespace_part_tags_date_2023 PARTITION OF tablespace_part_tags FOR VALUES FROM ('2023-01-01') TO ('2023-12-31') TABLESPACE ts_b;
CREATE TABLE
forumdb=> CREATE TABLE tablespace_part_tags_date_default PARTITION OF tablespace_part_tags default;
CREATE TABLE
```

现在，让我们插入一些数据：

```
forumdb=> insert into tablespace_part_tags (tag,ins_date,level) values ('Operating Systems','2022-01-01',0), ('Linux','2022-02-01',1),('BSD Unix','2023-03-01',1),('Rocky Linux Distro','2018-04-01',2);
INSERT 0 4
```

然后，让我们看看记录存储在哪里：

```
forumdb=> select * from tablespace_part_tags;
pk | ins_date | tag | level
----+------------+---------------+-------
 1 | 2022-01-01 | Operating Systems | 0
 2 | 2022-02-01 | Linux | 1
 3 | 2023-03-01 | BSD Unix | 1
 4 | 2018-04-01 | Rocky Linux Distro | 2
(4 rows)

forumdb=> select * from tablespace_part_tags_date_2022 ;
pk | ins_date | tag | level
```





-----+-----------+---------------+-------
 1 | 2022-01-01 | Operating Systems |   0
 2 | 2022-02-01 | Linux           |   1
(2 rows)

forumdb=> select * from tablespace_part_tags_date_2023 ;
pk | ins_date | tag | level
-----+-----------+---------------+-------
 3 | 2023-03-01 | BSD Unix |   1
(1 row)

forumdb=>select * from tablespace_part_tags_date_default;
pk | ins_date | tag | level
-----+-----------+---------------+-------
 4 | 2018-04-01 | Rocky Linux Distro |   2
(1 row)

正如我们在本练习中所看到的，数据已被拆分到不同的表空间中，因此我们将速度翻倍。这是一种非常有效的技术。

## 一个简单的案例研究

在本节的最后，我们将不使用 forumdb 数据库。我们将使用的是名为 world_temperatures 的数据库，其公开数据已从 https://www.meteoblue.com/it/tempo/archive/export 上的公开 CSV 导入。

db-world-temperatures 数据库备份可以在 packtpub GitHub 的第 9 章目录中找到，文件名为 backup-db-world-temperatures.sql.gz。如果你使用的是 Docker 镜像，你将已经拥有所有可用内容；否则，要导入数据库，请在你的服务器上运行 PostgreSQL：

```
$ gunzip < backup-db-world-temperatures.sql.gz | psql
```

如果你使用的是 Docker 镜像，只需执行以下操作：

```
postgres@learn_postgresql:~$ psql -U postgres world_temperatures
```

现在，db-world-temperatures 数据库已准备就绪。在数据库中，你会找到一个未分区的表 basilea 和一个分区的表 basilea_partitioned；这两个表都包含从 1950 年到 2022 年对巴塞尔市的温度信息，按规则的每小时间隔采样。现在，让我们看看在搜索分区表和非分区表之间的行为差异。在继续练习之前，如果你还不熟悉 EXPLAIN 语句的行为，请查看第 13 章《EXPLAIN 语句》。

让我们先使用非分区表，并编写一个查询，其结果返回从 1950 年开始的期间内 5 个最冷年份的平均温度：

```
world_temperatures=# select extract (year from insert_time) as year,
avg(temperature) avg_temp  from basilea group by 1 order by 2  limit 5;
year |     avg_temp
--------+---------------
1956 | 8.8073832344034608
1963 | 8.9077977708904110
1980 | 9.3459840948315118
1969 | 9.3705488990867580
1972 | 9.3749401615437158
(5 rows)
```

同样，让我们获取 5 个最暖年份：

```
world_temperatures=# select extract (year from insert_time) as year,
avg(temperature) avg_temp  from basilea group by 1 order by 2 desc limit 5;
year |     avg_temp
--------+---------------
2022 | 12.7320820592465753
2018 | 12.5638742964611872
2020 | 12.3662106902322404
2014 | 12.0601722329908676
2015 | 11.9246379973744292
(5 rows)
```

让我们以最后一个为例，看看其内部是如何完成的：

```
world_temperatures=# explain analyze select extract (year from insert_time) as year, avg(temperature) avg_temp  from basilea group by 1 order by 2 desc limit 5;

QUERY PLAN

------------------------------------------------------------
------------------------------------------------------------
Limit  (cost=98293.21..98293.23 rows=5 width=64) (actual
time=380.284..380.286 rows=5 loops=1)
  ->  Sort  (cost=98293.21..99892.99 rows=639912 width=64) (actual
time=380.282..380.283 rows=5 loops=1)
        Sort Key: (avg(temperature)) DESC
        Sort Method: top-N heapsort  Memory: 25kB
      ->  HashAggregate  (cost=68067.20..87664.51 rows=639912 width=64)
(actual time=380.121..380.253 rows=73 loops=1)
        Group Key: EXTRACT(year FROM insert_time)
        Planned Partitions: 32  Batches: 1  Memory Usage: 817kB
      ->  Seq Scan on basilea  (cost=0.00..12074.90 rows=639912
width=40) (actual time=0.030..189.680 rows=639912 loops=1)
Planning Time: 0.170 ms
Execution Time: 380.480 ms
(10 rows)
```

正如我们所见，PostgreSQL 对整个表执行了顺序扫描。现在考虑分区表 basilea_partitioned：

```
world_temperatures=# \d+ basilea_partitioned

Partitioned

table "public.basilea_partitioned"
    Column      |      Type       | [...] 
----------------+-----------------+--------
id              | integer         | [...] 
insert_time     | timestamp with time zone | [...]
```

```
temperature | numeric(8,6)             | [...]

Partition key: RANGE (insert_time)
Indexes:
    "basilea_partitioned_pkey" PRIMARY KEY, btree (id, insert_time)
Partitions: basilea_partitioned_1950 FOR VALUES FROM ('1949-12-31
23:00:00+00') TO ('1950-12-31 23:00:00+00'),
            basilea_partitioned_1951 FOR VALUES FROM ('1950-12-31
23:00:00+00') TO ('1951-12-31 23:00:00+00'),
[...]
            basilea_partitioned_2023 FOR VALUES FROM ('2022-12-31
23:00:00+00') TO ('2023-12-31 23:00:00+00'),
            basilea_partitioned_default DEFAULT
```

该表按年份从 1950 年到 2022 年进行分区，并且还有一个默认表（如“默认分区”部分所述）。

数据在所有子表之间平均分配，如果我们尝试执行相同的查询，我们将得到以下结果：

```sql
world_temperatures=# explain analyze select extract (year from insert_time) as year, avg(temperature) avg_temp  from basilea_partitioned group by 1 order by 2 desc limit 5;

QUERY PLAN

------------------------------------------------------------------------
------------------------------------------------------------------------
--------------------------------------------
 Limit  (cost=13183.59..13183.61 rows=5 width=64) (actual time=169.996..174.092 rows=5 loops=1)
   ->  Sort  (cost=13183.59..13184.09 rows=200 width=64) (actual time=169.995..174.090 rows=5 loops=1)
         Sort Key: (avg(basilea_partitioned.temperature)) DESC
         Sort Method: top-N heapsort  Memory: 25kB
         ->  Finalize GroupAggregate  (cost=13127.60..13180.27 rows=200 width=64) (actual time=169.812..174.060 rows=73 loops=1)
               Group Key: (EXTRACT(year FROM basilea_partitioned.insert_time))
```

```
-> Gather Merge  (cost=13127.60..13174.27 rows=400
width=64) (actual time=169.802..173.941 rows=132 loops=1)
        Workers Planned: 2
        Workers Launched: 2
        -> Sort  (cost=12127.58..12128.08 rows=200 width=64)
(actual time=140.386..140.401 rows=44 loops=3)
                Sort Key: (EXTRACT(year FROM basilea_
partitioned.insert_time))
                Sort Method: quicksort  Memory: 30kB
                Worker 0:  Sort Method: quicksort  Memory: 29kB
                Worker 1:  Sort Method: quicksort  Memory: 29kB
        -> Partial HashAggregate
(cost=12116.93..12119.93 rows=200 width=64) (actual time=140.330..140.363
rows=44 loops=3)
                Group Key: (EXTRACT(year FROM basilea_
partitioned.insert_time))
                Batches: 1  Memory Usage: 64kB
                Worker 0:  Batches: 1  Memory Usage: 48kB
                Worker 1:  Batches: 1  Memory Usage: 64kB
        -> Parallel Append  (cost=0.00..10780.64
rows=267259 width=40) (actual time=0.010..79.251 rows=213304 loops=3)
        -> Parallel Seq Scan on basilea_
partitioned_1952 basilea_partitioned_3  (cost=0.00..129.59 rows=5167
width=40) (actual time=0.010..3.176 rows=8784 loops=1)
[...]
        -> Parallel Seq Scan on basilea_
partitioned_2018 basilea_partitioned_69  (cost=0.00..128.41 rows=5153
width=40) (actual time=0.004..2.671 rows=8760 loops=1)
        -> Parallel Seq Scan on basilea_
partitioned_2021 basilea_partitioned_72  (cost=0.00..128.41 rows=5153
width=40) (actual time=0.003..2.651 rows=8760 loops=1)
        -> Parallel Seq Scan on basilea_
partitioned_default basilea_partitioned_75  (cost=0.00..21.10 rows=888
width=40) (actual time=0.000..0.000 rows=0 loops=1)
        -> Parallel Seq Scan on basilea_
partitioned_2023 basilea_partitioned_74  (cost=0.00..1.01 rows=1 width=40)
(actual time=0.008..0.009 rows=1 loops=1)
Planning Time: 0.698 ms
```



Execution Time: 174.250 ms
(97 rows)

正如我们所见，PostgreSQL 首先执行并行顺序扫描，然后执行并行追加以合并从子表获取的所有数据。

现在让我们尝试执行相同的操作，但过滤年份范围为 2021 年至 2022 年。在非分区表上我们将有以下内容：

```
world_temperatures=# explain analyze select extract (year from insert_time) as year, avg(temperature) avg_temp from basilea where insert_time >= '2021-01-01' and insert_time < '2023-01-01' group by 1 order by 2 desc limit 5;

QUERY PLAN

---------------------------------------------------------------
 Limit  (cost=11498.05..11498.06 rows=5 width=64) (actual time=24.532..28.875 rows=2 loops=1)
   ->  Sort  (cost=11498.05..11544.43 rows=18554 width=64) (actual time=24.529..28.871 rows=2 loops=1)
         Sort Key: (avg(temperature)) DESC
         Sort Method: quicksort  Memory: 25kB
       ->  Finalize HashAggregate  (cost=10911.56..11189.87 rows=18554 width=64) (actual time=24.467..28.859 rows=2 loops=1)
             Group Key: (EXTRACT(year FROM insert_time))
             Batches: 1  Memory Usage: 793kB
           ->  Gather  (cost=9133.43..10795.60 rows=15462 width=64) (actual time=24.159..28.707 rows=6 loops=1)
                 Workers Planned: 2
                 Workers Launched: 2
               ->  Partial HashAggregate  (cost=8133.43..8249.40 rows=7731 width=64) (actual time=20.170..20.211 rows=2 loops=3)
                     Group Key: EXTRACT(year FROM insert_time)
                     Batches: 1  Memory Usage: 409kB
                     Worker 0:  Batches: 1  Memory Usage: 409kB
                     Worker 1:  Batches: 1  Memory Usage: 409kB
                   ->  Parallel Seq Scan on basilea (cost=0.00..8094.78 rows=7731 width=40) (actual time=16.211..18.330 rows=5840 loops=3)
```

```
Filter: ((insert_time >= '2021-01-01 00:00:00+00'::timestamp with time zone) AND (insert_time < '2023-01-01 00:00:00+00'::timestamp with time zone))
Rows Removed by Filter: 207464
Planning Time: 0.248 ms
Execution Time: 30.043 ms
(20 rows)
```

对于分区表，我们将有以下内容：

```
world_temperatures=# explain analyze select extract (year from insert_time) as year, avg(temperature) avg_temp  from basilea_partitioned where insert_time >='2021-01-01' and insert_time < '2023-01-01' group by 1 order by 2 desc limit 5;

QUERY PLAN

-----------------------------------------
Limit  (cost=618.10..618.11 rows=5 width=64) (actual time=15.205..15.208 rows=2 loops=1)
  ->  Sort  (cost=618.10..618.60 rows=200 width=64) (actual time=15.203..15.205 rows=2 loops=1)
        Sort Key: (avg(basilea_partitioned.temperature)) DESC
        Sort Method: quicksort  Memory: 25kB
      ->  HashAggregate  (cost=611.78..614.78 rows=200 width=64)
(actual time=15.190..15.194 rows=2 loops=1)
        Group Key: (EXTRACT(year FROM basilea_partitioned.insert_time))
        Batches: 1  Memory Usage: 40kB
          ->  Append  (cost=0.00..524.19 rows=17517 width=40) (actual time=0.032..8.804 rows=17520 loops=1)
              ->  Seq Scan on basilea_partitioned_2021
basilea_partitioned_1  (cost=0.00..217.30 rows=8758 width=40) (actual time=0.030..4.466 rows=8759 loops=1)
        Filter: ((insert_time >= '2021-01-01 00:00:00+00'::timestamp with time zone) AND (insert_time < '2023-01-01 00:00:00+00'::timestamp with time zone))
Rows Removed by Filter: 1
```

```
enumvals       | {partition,on,off}
boot_val       | partition
reset_val      | partition
sourcefile     |

sourceline     |
pending_restart | f
```

该参数使得查询优化器可以从搜索中排除某些子表。正如你在前面的代码中所见，`constraint_exclusion` 参数的可能取值如下：

+   - **on:** 设置此值时，PostgreSQL 会检查所有表。
+   - **off:** 设置此值时，PostgreSQL 不会检查任何约束。
+   - **partition:** 使用此值时，PostgreSQL 会检查 UNION ALL 子查询的约束，仅针对继承子表。`partition` 是默认设置。

有关更多信息，请参阅 https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-CONSTRAINT-EXCLUSION。

## Summary

在本章中，我们介绍了 PostgreSQL 中表分区的主题。随着表变得越来越大，分区表很有用，因为查询会变得越来越慢。我们首先介绍了分区的概念。我们讨论了范围分区、列表分区和哈希分区。我们还通过使用表空间的示例介绍了列表分区和范围分区。

我们将在 *第 13 章，索引和性能优化* 中再次讨论分区。在下一章中，我们将讨论 PostgreSQL 如何管理用户、角色，以及总体上数据库的安全性。

## Verify your knowledge

+   - 是否可以在 PostgreSQL 中执行声明式分区？

是的，从 PostgreSQL 10 开始，可以使用声明式分区。

请参阅 *探索声明式分区* 部分了解更多详情。

+   - 如果我们有一个如下的表：

```
CREATE TABLE mytable (
    pk serial NOT NULL,
    create_date date not null default now()::date,
    primary key (pk)
)
```

+   - 是否可以按范围对该表进行分区？是的，可以通过编写如下内容来实现：

```
CREATE TABLE mytable (
    pk serial NOT NULL,
    create_date date not null default now()::date,
    primary key (pk,creat_edate)
)PARTITION BY RANGE (create_date);
CREATE TABLE

forumdb=> CREATE TABLE part_tags_date_01 PARTITION OF part_tags FOR
VALUES FROM ('2023-01-01') TO ('2023-06-31');
forumdb=> CREATE TABLE part_tags_date_01 PARTITION OF part_tags FOR
VALUES FROM ('2023-07-01') TO ('2023-12-31');
```

请参阅 *探索声明式分区* 部分了解更多详情。

+   - 默认分区是什么？

默认分区确保不会丢失数据；如果 PostgreSQL 找不到存储记录的子表，则记录将保存在默认分区中。

请参阅 *默认分区* 部分了解更多详情。

+   - 我们可以将数据拆分到不同的磁盘上吗？

是的，可以使用表空间。

请参阅 *分区和表空间* 部分了解更多详情。

+   - PostgreSQL 是否管理分区表上的索引？

是的，PostgreSQL 管理分区表上的索引；如果我们在父表上构建索引，PostgreSQL 将负责在所有子表上构建索引。

## References

+   - PostgreSQL 官方文档关于表分区：https://www.postgresql.org/docs/current/ddl-partitioning.html
+   - PostgreSQL 官方文档关于继承：https://www.postgresql.org/docs/current/tutorial-inheritance.html
+   - PostgreSQL 调优：https://pgtune.leopard.in.ua
+   - PostgreSQL 官方文档关于 CONSTRAINT EXCLUSION：https://www.postgresql.org/docs/current/runtime-config-query.html#GUC-CONSTRAINT-EXCLUSION
+   - PostgreSQL 官方文档关于三元组：https://www.postgresql.org/docs/current/pgtrgm.html

## Learn more on Discord

要加入本书的 Discord 社区——在那里你可以分享反馈、向作者提问并了解新版本——请扫描以下二维码：

https://discord.gg/jYWCjF6Tku

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_337_0.png)
