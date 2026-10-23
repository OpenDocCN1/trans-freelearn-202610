
```
Index Cond: (author = u.pk)
Planning Time: 0.401 ms
Execution Time: 0.954 ms
(12 rows)
```

首先，查询现在是顺序执行的，因为没有Gather节点：这意味着PostgreSQL现在能够提前减少需要检查的结果集。此外，执行时间现在不到一毫秒。

如你所见，第一个执行的节点是对posts表的Index Scan节点，与之前的并行计划不同，它现在被首先使用，然后通过Nested Loop与authors表连接缩减后的结果集。使用Nested Loop证实了PostgreSQL能够提前从posts表中只获取所需的元组，从而缩减结果集。

同样有趣的是，不再有任何理由保留created_on列上的索引，因为前面的查询计划已经不再使用它了。

现在索引需要多少空间？同样，检查起来非常简单：

```
forumdb=> SELECT pg_size_pretty( pg_relation_size( 'posts') ) AS table_size,
    pg_size_pretty( pg_relation_size( 'idx_posts_date' ) ) AS idx_date_size,
    pg_size_pretty( pg_relation_size( 'idx_posts_author' ) ) AS idx_author_size;

table_size | idx_date_size | idx_author_size
------------+---------------+----------------
59 MB      | 3600 kB       | 3600 kB

(1 row)
```

两个索引占用相同的空间，但正如我们所说，我们可以删除基于日期的索引，因为它不再需要。事实上，即使有特定的日期条件，该索引也不再被使用：

```
forumdb=> EXPLAIN ANALYZE
    SELECT p.title, u.username
    FROM posts p
    JOIN users u ON u.pk = p.author
    WHERE u.username = 'fluca1978'
AND  p.created_on::date = CURRENT_DATE -2;

QUERY PLAN
--------------------------------------------------
Nested Loop (cost=8.45..1599.79 rows=2 width=26) (actual time=0.132..0.737 rows=1 loops=1)
  ->  Index Scan using users_username_key on users u  (cost=0.28..8.29 rows=1 width=19) (actual time=0.009..0.010 rows=1 loops=1)
        Index Cond: (username = 'fluca1978'::text)
  ->  Bitmap Heap Scan on posts p  (cost=8.17..1591.48 rows=2 width=15) (actual time=0.119..0.723 rows=1 loops=1)
        Recheck Cond: (author = u.pk)
        Filter: ((created_on)::date = (CURRENT_DATE - 2))
        Rows Removed by Filter: 499
        Heap Blocks: exact=500
      ->  Bitmap Index Scan on idx_posts_author  (cost=0.00..8.17 rows=500 width=0) (actual time=0.053..0.053 rows=500 loops=1)
            Index Cond: (author = u.pk)
Planning Time: 0.237 ms
Execution Time: 0.763 ms
```

识别未使用的索引很重要，因为它能让我们回收磁盘空间，并简化管理和数据插入：请记住，每次表发生变化时，索引都必须更新，这也会消耗额外资源，例如时间和磁盘空间。

因此，如你所见，分析应用程序执行最频繁的查询并判断索引是否有助于提高执行速度非常重要，但同时也要记住，索引在空间和维护方面都有额外成本，所以不要滥用索引。

但是，如果连正在运行的查询都不知道，你又如何识别未使用的索引呢？

幸运的是，PostgreSQL为你提供了关于每个索引使用情况的详细信息：特殊的pg_stat_user_indexes视图提供了索引被使用了多少次以及如何使用的信息。例如，要获取posts表上索引的信息，你可以执行如下命令：

```
forumdb=> SELECT indexrelname, idx_scan, idx_tup_read, idx_tup_fetch FROM pg_stat_user_indexes WHERE relname = 'posts';
```

```
indexrelname   | idx_scan | idx_tup_read | idx_tup_fetch
---------------+----------+--------------+--------------
posts_pkey     |        5 |            5 |            5
idx_posts_date |        1 |       500000 |       500000
idx_posts_author |     51 |         8534 |            0
```

这告诉我们，idx_posts_date被使用了1次，提供了500000个元组，而idx_posts_author被使用了51次，提供的元组要少得多（仅8534个），这意味着它非常高效。

随着时间推移，如果观察到这种趋势，即idx_posts_date很少被使用，你就可以放心地删除它。

作为最后一个例子，让我们考虑一个写得不好的查询及其隐含的问题：假设我们想要提取在某个日期创建了帖子，并且在同一天获得了特定数量点赞的所有作者。一个糟糕的查询可能是这样的：

```sql
SELECT u.username
FROM users u JOIN posts p ON p.author = u.pk WHERE p.created_on = CURRENT_DATE - 5
AND u.pk IN ( SELECT pp.author FROM posts pp WHERE likes = 5 and
p.created_on = created_on );
```

显然，这里并不需要对posts表的子查询，但这是一个具体的例子，用来演示理解如何编写良好查询的重要性。

乍一看，前面的查询运行得足够快；它执行需要190毫秒，正如EXPLAIN ANALYZE所展示的那样：

```text
forumdb=> EXPLAIN ANALYZE SELECT u.username
FROM users u JOIN posts p ON p.author = u.pk WHERE p.created_on = CURRENT_DATE - 5
AND u.pk IN ( SELECT pp.author FROM posts pp WHERE likes = 5 and
p.created_on = created_on );
                              QUERY PLAN

--------------------------------------------------------------------------

Hash Join  (cost=33.93..92.04 rows=498 width=15) (actual
 time=1.351..189.308 rows=100 loops=1)
   Hash Cond: (p.author = u.pk)
```
```
Join Filter: (SubPlan 1)
Rows Removed by Join Filter: 900
->  Index Scan using idx_posts_date on posts p  (cost=0.43..55.92 rows=996 width=12) (actual time=0.029..0.254 rows=1000 loops=1)
    Index Cond: (created_on = (CURRENT_DATE - 5))
->  Hash  (cost=21.00..21.00 rows=1000 width=19) (actual time=0.345..0.345 rows=1000 loops=1)
    Buckets: 1024  Batches: 1  Memory Usage: 59kB
    ->  Seq Scan on users u  (cost=0.00..21.00 rows=1000 width=19) (actual time=0.006..0.151 rows=1000 loops=1)
SubPlan 1
    ->  Index Scan using idx_posts_date on posts pp  (cost=0.42..58.49 rows=99 width=4) (actual time=0.005..0.176 rows=95 loops=1000)
        Index Cond: (created_on = p.created_on)
        Filter: (likes = 5)
        Rows Removed by Filter: 855
Planning Time: 0.393 ms
Execution Time: 189.439 ms
```

然而，查看输出，你可以看到在pp posts上使用idx_posts_date的Index Scan节点的loops计数器设置为1000：这意味着该节点在查询中执行了1000次。该节点就是子查询，这正是SQL实现结果的方式：每当找到外层查询的一个元组，就必须运行内层查询并连接结果。一种可能的方案是重写查询，避免PostgreSQL进行循环，例如，使用带有GROUP BY表达式的子查询：

```
forumdb=> EXPLAIN ANALYZE
WITH likes AS (
    SELECT pp.author, pp.created_on FROM posts pp
    WHERE likes = 5
    GROUP BY pp.author, pp.created_on
)
SELECT u.username
FROM users u JOIN posts p ON p.author = u.pk
JOIN likes l ON l.created_on = p.created_on AND l.author = u.pk
WHERE p.created_on = CURRENT_DATE - 5;
```

```
---------------------------------------------
---------------------------------------------
-------------
Hash Join  (cost=89.47..149.68 rows=98 width=15) (actual time=0.581..0.896 rows=100 loops=1)
  Hash Cond: (p.author = u.pk)
  ->  Index Scan using idx_posts_date on posts p  (cost=0.43..55.92 rows=996 width=12) (actual time=0.012..0.167 rows=1000 loops=1)
        Index Cond: (created_on = (CURRENT_DATE - 5))
  ->  Hash  (cost=87.82..87.82 rows=98 width=31) (actual time=0.561..0.562 rows=100 loops=1)
        Buckets: 1024  Batches: 1  Memory Usage: 15kB
        ->  Hash Join  (cost=62.09..87.82 rows=98 width=31) (actual time=0.305..0.540 rows=100 loops=1)
              Hash Cond: (u.pk = l.author)
              ->  Seq Scan on users u  (cost=0.00..21.00 rows=1000 width=19) (actual time=0.005..0.094 rows=1000 loops=1)
              ->  Hash  (cost=60.86..60.86 rows=98 width=12) (actual time=0.294..0.294 rows=100 loops=1)
                    Buckets: 1024  Batches: 1  Memory Usage: 13kB
                    ->  Subquery Scan on l  (cost=58.90..60.86 rows=98 width=12) (actual time=0.243..0.276 rows=100 loops=1)
                          ->  HashAggregate  (cost=58.90..59.88 rows=98 width=12) (actual time=0.242..0.260 rows=100 loops=1)
                                Group Key: pp.author, pp.created_on
                                Batches: 1  Memory Usage: 24kB
                                ->  Index Scan using idx_posts_date on posts pp  (cost=0.43..58.41 rows=98 width=12) (actual time=0.009..0.208 rows=100 loops=1)
                                      Index Cond: (created_on = (CURRENT_DATE - 5))
                                      Filter: (likes = 5)
                                      Rows Removed by Filter: 900
Planning Time: 0.455 ms
Execution Time: 0.954 ms
```

