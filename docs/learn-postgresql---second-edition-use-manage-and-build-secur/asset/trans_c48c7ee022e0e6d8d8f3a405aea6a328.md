

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

# 10

# Users, Roles, and Database Security

PostgreSQL 是一个坚如磐石的数据库，对安全性给予了极大的关注，提供了非常丰富的用于处理权限、特权和安全策略的基础设施。本章基于第 3 章《管理用户和连接》中介绍的基本概念，重新审视角色概念，并扩展知识，特别关注授予角色的安全性和特权（角色可以是用户，也可以是用户组）。你将学习如何配置角色的各个方面，以仔细管理安全性，从连接到数据库内数据的访问。

PostgreSQL 还提供了一种强大的机制，即行级安全性（RLS），它允许对策略进行精细定义，以屏蔽某些用户的数据部分。

在本章中，你还将了解访问控制列表（ACL）以及 PostgreSQL 在内部处理权限的方式，这是授予或撤销特权的结果。最后，你将简要了解 PostgreSQL 提供的用于安全存储角色密码的密码加密算法。

本章涵盖以下主题：

+   - 理解角色
+   - ACLs
+   - 授予和撤销权限
+   - RLS
+   - 角色密码加密
+   - SSL 连接

