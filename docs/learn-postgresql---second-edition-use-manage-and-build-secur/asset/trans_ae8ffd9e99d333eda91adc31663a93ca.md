

-------------+-----------------+---------
500000 | 59 MB           | posts
1000   | 88 kB            | users

本章的 Docker 镜像已预先填充了上述数量的数据。请注意谨慎，因为此类数据需要您大约有 60 MB 的可用磁盘空间。

让我们从一个简单的示例开始；我们想要按创建日期提取所有帖子，因此查询如下：

```
SELECT * FROM posts ORDER BY created_on;
```

我们可以将其传递给 EXPLAIN 来了解 PostgreSQL 将如何执行它：

```
forumdb=> EXPLAIN SELECT * FROM posts ORDER BY created_on;
                 QUERY PLAN
--------------------------------------------------------------------------------------------------------------------------------
Sort  (cost=83838.92..85088.92 rows=500000 width=81)
  Sort Key: created_on
  ->  Seq Scan on posts  (cost=0.00..12584.00 rows=500000 width=81)
```

正如你所见，第一个要执行的节点是顺序扫描（初始成本为 0），它将产生 500,000 个元组作为输出。为什么是顺序扫描？首先，没有过滤条件——我们想要检索表中存储的所有数据——其次，表上没有访问方法（没有索引）。

由于我们要求对输出进行排序，下一个要执行的节点是排序节点，它产生的元组数量与结果完全相同。

执行前面的查询需要多长时间？EXPLAIN ANALYZE 可以帮助我们回答这个问题：

```
forumdb=> EXPLAIN ANALYZE SELECT * FROM posts ORDER BY created_on;
--------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------
Sort  (cost=83838.92..85088.92 rows=500000 width=81) (actual time=304.283..393.395 rows=500000 loops=1)
  Sort Key: created_on
```

```
Sort Method: external merge  Disk: 51192kB
->  Seq Scan on posts  (cost=0.00..12584.00 rows=500000 width=81)
(actual time=0.045..63.449 rows=500000 loops=1)
Planning Time: 0.059 ms
Execution Time: 429.838 ms
```

纯执行时间接近半秒。通过在 created_on 字段上构建特定索引是否可以减少总时间：

```
forumdb=> CREATE INDEX idx_posts_date ON posts( created_on );
CREATE INDEX
forumdb=> EXPLAIN ANALYZE SELECT * FROM posts ORDER BY created_on;

QUERY PLAN
--------------------------------------------------
--------------------------------------------------
Index Scan using idx_posts_date on posts  (cost=0.42..16887.80 rows=500000 width=81) (actual time=0.079..133.305 rows=500000 loops=1)
Planning Time: 0.203 ms
Execution Time: 162.143 ms
(3 rows)
```

查询现在以几乎是无索引时所需时间的三分之一的速度运行，并且事实上，查询计划已从顺序扫描更改为使用新创建的新索引的索引扫描。

当然，新创建的索引在存储空间方面有惩罚；正如你所想象的，速度的提升伴随着额外的空间成本，可以通过以下方式检查：

```
forumdb=> SELECT pg_size_pretty( pg_relation_size( 'posts' ) ) AS table_size,
    pg_size_pretty( pg_relation_size( 'idx_posts_date' ) ) AS index_size;
    table_size | index_size
----------------+----------
59 MB          | 3600 kB
(1 row)
```

现在，是否额外的磁盘空间太多取决于你的资源和最终目标；在前面的情况下，假设你经常执行查询，增加的速度是合理的。

让我们现在集中在更典型的查询上：查找特定用户在特定时间段内的所有帖子。假设为 2 天时间段，结果查询将如下所示：

```
SELECT p.title, u.username
FROM posts p
JOIN users u ON u.pk = p.author
WHERE u.username = 'fluca1978'
AND daterange( CURRENT_DATE - 2, CURRENT_DATE ) @> p.created_on::date
```

PostgreSQL 如何执行前面的查询？同样，EXPLAIN 可以帮助我们了解数据库认为的最佳查询计划是什么：

```
forumdb=> EXPLAIN
    SELECT p.title, u.username
    FROM posts p
    JOIN users u ON u.pk = p.author
    WHERE u.username = 'fluca1978'
    AND daterange( CURRENT_DATE - 2, CURRENT_DATE ) @> p.created_on::date;

                              QUERY PLAN
----------------------------------------------------------------------------------------------------------------------------------------
Gather  (cost=1008.30..13803.58 rows=2 width=26)
  Workers Planned: 2
  ->  Hash Join  (cost=8.30..12803.38 rows=1 width=26)
        Hash Cond: (p.author = u.pk)
        ->  Parallel Seq Scan on posts p  (cost=0.00..12792.33 rows=1042 width=15)
              Filter: (daterange((CURRENT_DATE - 2), CURRENT_DATE) @> (created_on)::date)
        ->  Hash  (cost=8.29..8.29 rows=1 width=19)
              ->  Index Scan using users_username_key on users u (cost=0.28..8.29 rows=1 width=19)
                    Index Cond: (username = 'fluca1978'::text)
```

规划器选择启动并行查询执行：顶部节点是 Gather 节点，因此充当并行工作者的同步点。特别是，规划器决定使用两个并行进程来完成查询。最右边的节点，因此是第一个被执行的节点，是 users 表上的 Index Scan 节点。结果被哈希，并且与此同时，Parallel Seq Scan 被触发以从 posts 表中提取数据。结果被连接，Gather 收集最终结果集。此查询的总执行时间可以通过 EXPLAIN ANALYZE 轻松获得，它向我们展示查询完成大约需要 140 毫秒：

```
forumdb=> EXPLAIN ANALYZE
    SELECT p.title, u.username
    FROM posts p
    JOIN users u ON u.pk = p.author
    WHERE u.username = 'fluca1978'
    AND daterange( CURRENT_DATE - 20, CURRENT_DATE ) @> p.created_on::date;

QUERY PLAN
--------------------------------------------------
--------------------------------------------------
Gather  (cost=1008.30..13803.58 rows=2 width=26) (actual time=0.856..141.434 rows=20 loops=1)
  Workers Planned: 2
  Workers Launched: 2
  ->  Hash Join  (cost=8.30..12803.38 rows=1 width=26) (actual time=0.968..135.209 rows=7 loops=3)
        Hash Cond: (p.author = u.pk)
        ->  Parallel Seq Scan on posts p  (cost=0.00..12792.33 rows=1042 width=15) (actual time=0.076..133.975 rows=6667 loops=3)
              Filter: (daterange((CURRENT_DATE - 20), CURRENT_DATE) @> (created_on)::date)
              Rows Removed by Filter: 160000
        ->  Hash  (cost=8.29..8.29 rows=1 width=19) (actual time=0.053..0.054 rows=1 loops=3)
              Buckets: 1024  Batches: 1  Memory Usage: 9kB
```

```
->  Index Scan using users_username_key on users u
(cost=0.28..8.29 rows=1 width=19) (actual time=0.047..0.048 rows=1 loops=3)

    Index Cond: (username = 'fluca1978'::text)

Planning Time: 0.188 ms
Execution Time: 141.471 ms
```

如果我们在 posts 表的 author 列上添加索引会怎样？

```
forumdb=> CREATE INDEX idx_posts_author ON posts( author );
CREATE INDEX
forumdb=> EXPLAIN ANALYZE
    SELECT p.title, u.username
    FROM posts p
    JOIN users u ON u.pk = p.author
    WHERE u.username = 'fluca1978'
    AND  daterange( CURRENT_DATE - 20, CURRENT_DATE ) @> p.created_on::date;

QUERY PLAN
---------------------------------------------------------------
---------------------------------------------------------------
Nested Loop  (cost=8.45..1602.29 rows=2 width=26) (actual time=0.145..0.926 rows=20 loops=1)
  ->  Index Scan using users_username_key on users u  (cost=0.28..8.29 rows=1 width=19) (actual time=0.011..0.012 rows=1 loops=1)
         Index Cond: (username = 'fluca1978'::text)
  ->  Bitmap Heap Scan on posts p  (cost=8.17..1593.98 rows=2 width=15) (actual time=0.129..0.904 rows=20 loops=1)
         Recheck Cond: (author = u.pk)
         Filter: (daterange((CURRENT_DATE - 20), CURRENT_DATE) @> (created_on)::date)
         Rows Removed by Filter: 480
         Heap Blocks: exact=500
           ->  Bitmap Index Scan on idx_posts_author  (cost=0.00..8.17 rows=500 width=0) (actual time=0.059..0.059 rows=500 loops=1)
```
