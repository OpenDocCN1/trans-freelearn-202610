

- auto_explain.log_timing，开启后，提供每个节点的计时信息。
- auto_explain.log_triggers，开启后，提供语句中触发器执行的信息。
- auto_explain.log_settings，开启后，会报告与集群级配置不同的设置。

为了安装和配置该模块，我们从一个简单的开始，在 postgresql.conf 的集群配置中添加以下两个设置：

```
session_preload_libraries = 'auto_explain'
auto_explain.log_min_duration = '100ms'
```

第一行告诉 PostgreSQL 加载与 auto-explain 模块相关的库，而第二行则让模块在任何查询耗时超过 100 毫秒时触发记录。当然，你可以根据自己的需求调高或调低查询时长阈值。

完成配置后，现在可以执行一个较长的查询，如下所示（假设你已经删除了上一节中创建的索引或禁用了它们）：

```
forumdb=> \timing
forumdb=> SELECT count(*)
FROM posts p
JOIN users u ON u.pk = p.author
WHERE u.username = 'fluca1978'
AND daterange( CURRENT_DATE - 20, CURRENT_DATE ) @> p.created_on::date;
 count
-------
     20
(1 row)

Time: 142.629 ms
```

该查询花费了 142 毫秒，足以触发我们的 auto-explain 功能。事实上，在 PostgreSQL 日志中，你可以看到以下内容：

```
$ tail /postgres/16/data/log/postgresql.log
INFO:  duration: 139.933 ms  plan:
Query Text: SELECT count(*)
FROM forum.posts p
JOIN forum.users u ON u.pk = p.author
WHERE u.username = 'fluca1978'
AND  daterange( CURRENT_DATE - 20, CURRENT_DATE ) @> p.created_on::date;
Aggregate  (cost=13803.59..13803.60 rows=1 width=8)
    Output: count(*)
  ->  Gather  (cost=1008.30..13803.58 rows=2 width=0)
        Workers Planned: 2
      ->  Hash Join  (cost=8.30..12803.38 rows=1 width=0)
            Inner Unique: true
            Hash Cond: (p.author = u.pk)
          ->  Parallel Seq Scan on forum.posts p  (cost=0.00..12792.33 rows=1042 width=4)
...
```

这正是普通 `EXPLAIN` 命令对同一查询所产生的输出。

这种方法的美妙之处在于，你不必担心或记住要对查询或收集到的查询执行 `EXPLAIN`；你只需检查日志，就能找出慢查询的执行计划。一旦你修复了如上所示的查询（例如通过创建索引），你可以提高 auto-explain 的阈值来捕获更慢的查询，并再次重复该过程。

为了演示开启 auto_explain.log_analyze 后运行查询在输出上的差异，下面是针对同一查询所产生的输出：

```
INFO:  duration: 139.730 ms  plan:
Query Text: SELECT count(*)
FROM forum.posts p
JOIN forum.users u ON u.pk = p.author
WHERE u.username = 'fluca1978'
AND  daterange( CURRENT_DATE - 20, CURRENT_DATE ) @> p.created_on::date;
Aggregate  (cost=13803.59..13803.60 rows=1 width=8) (actual time=138.110..139.720 rows=1 loops=1)
    Output: count(*)
    Buffers: shared hit=487 read=7264
  ->  Gather  (cost=1008.30..13803.58 rows=2 width=0) (actual time=0.397..139.703 rows=20 loops=1)
        Workers Planned: 2
```

```
Workers Launched: 2
Buffers: shared hit=487 read=7264
  ->  Hash Join  (cost=8.30..12803.38 rows=1 width=0) (actual time=44.493..134.889 rows=7 loops=3)
    Inner Unique: true
    Hash Cond: (p.author = u.pk)
    Buffers: shared hit=487 read=7264
    Worker 0:  actual time=132.750..132.751 rows=0 loops=1
      Buffers: shared hit=238 read=3192
    Worker 1:  actual time=0.667..134.246 rows=4 loops=1
      Buffers: shared hit=162 read=2120
    ->  Parallel Seq Scan on forum.posts p  (cost=0.00..12792.33 rows=1042 width=4) (actual time=0.132..133.575 rows=6667 loops=3)
      Output: p.pk, p.title, p.content, p.author, p.category, p.reply_to, p.created_on, p.last_edited_on, p.editable, p.likes
      ;
  Finalize Aggregate  (cost=114848.33..114848.34 rows=1 width=8) (actual time=5190.322..5190.323 rows=1 loops=1)
    ->  Gather  (cost=114848.12..114848.33 rows=2 width=8) (actual time=5189.678..5193.226 rows=3 loops=1)
      Workers Planned: 2
      Workers Launched: 2
      ->  Partial Aggregate  (cost=113848.12..113848.13 rows=1 width=8) (actual time=4861.705..4861.712 rows=1 loops=3)
        ->  Hash Join  (cost=8.30..113848.09 rows=10 width=0) (actual time=2477.949..4861.639 rows=27 loops=3)
...
```

如你所见，输出现在包含了 `EXPLAIN ANALYZE` 所报告的相同信息。

## 总结

PostgreSQL 拥有一个非常复杂的基于成本的查询规划器和优化器，它会尽最大努力提供对底层数据的最快访问。

借助 `EXPLAIN` 命令，数据库管理员可以监控查询，追踪执行成本和耗时，并决定如何改进查询以获得更快的结果。通常，创建索引是查询调优中侵入性最小的选择，而 PostgreSQL 拥有非常丰富且富有表现力的索引接口，允许创建不同类型的单列、多列和部分索引，以及采用不同的技术。当索引不够用时，查询重写可能是执行查询调优的一种可行解决方案。

规划器使用的成本基于统计数据，而这些数据必须尽可能保持最新。虽然 auto-analyze 守护进程旨在完成这项工作，但 DBA 始终可以依靠手动执行 `ANALYZE` 命令来更新统计信息。

理解查询计划、了解涉及哪些节点以及它们对查询执行意味着什么、判断统计信息何时过期，以及能够尝试不同的查询访问方法，这些都是每个 DBA 应该学习的复杂任务。

我们还探讨了 auto-explain 扩展，它可以用于自动收集优化器所选计划的信息，从而让 DBA 轻松检查哪些查询运行不佳。

需要强调的是，性能调优是数据库管理中最复杂的任务之一，没有银弹或一刀切的解决方案，因此需要经验和大量实践。在下一章中，我们将通过尝试日志记录和审计来积累这方面的经验。

## 验证你的知识

- 如何检查查询的计划？
  
  专门的 `EXPLAIN` 命令允许你检查 PostgreSQL 将如何执行给定的查询，并为每个执行步骤显示一个“节点”。更多详情请参阅 *EXPLAIN 语句* 一节。
  
- `EXPLAIN` 和 `EXPLAIN ANALYZE` 之间有什么区别？
  
  `EXPLAIN` 命令不会执行查询，只计算访问计划；另一方面，`EXPLAIN ANALYZE` 命令会执行查询并在输出中打印查询计划。更多详情请参阅 *EXPLAIN ANALYZE* 一节。

- PostgreSQL 如何保持统计信息最新？
  
  每次手动执行 `ANALYZE` 命令或 auto-vacuum（auto-analyze）守护进程对表运行时，统计信息都会更新。更多详情请参阅 *ANALYZE 以及如何更新统计信息* 一节。
- PostgreSQL 如何选择使用特定的访问方法（例如索引）？
  
  优化器根据每种访问方法的成本来决定访问数据的路径：成本最低的方法获胜，并被用来访问底层数据。更多详情请参阅 *优化器* 一节。
- 什么是 auto_explain 扩展？
  
  auto_explain 扩展允许系统在查询达到定义的门槛（例如执行时间超过预设限制）时，自动将查询执行计划输出到日志中。这使 DBA 能够自动获取有关问题查询的信息。更多详情请参阅 *Auto-explain* 一节。

## 参考

- PostgreSQL 官方文档之 CREATE INDEX：https://www.postgresql.org/docs/current/sql-createindex.html
- PostgreSQL 官方文档之 pg_stats：https://www.postgresql.org/docs/current/view-pg-stats.html
- PostgreSQL 官方文档之 EXPLAIN：https://www.postgresql.org/docs/current/using-explain.html
- PostgreSQL 官方文档之 ANALYZE：https://www.postgresql.org/docs/current/sql-analyze.html
- Auto-explain 官方文档：https://www.postgresql.org/docs/current/auto-explain.html

