
```
Index Cond: (author = u.pk)
Planning Time: 0.401 ms
Execution Time: 0.954 ms
(12 rows)
```

First of all, the query now runs sequentially because there are no Gather nodes: this means that now PostgreSQL is able to reduce, in advance, the result set to inspect. Moreover, the execution time is now less than a millisecond.

As you can see, the first executed node is an Index Scan node on the posts table, which is now used first, unlike in the parallel previous plan, and the reduced result set is then joined by means of a Nested Loop with the authors table. The usage of a Nested Loop confirms that PostgreSQL has been able to reduce the result set obtaining in advance only the needed tuples from the posts table.

It is also interesting to note that there is no longer any reason to keep the index on the created_on column, since the preceding query plan is not using it anymore.

How much space is required for the indexes now? Again, it is quite simple to check:

```
forumdb=> SELECT pg_size_pretty( pg_relation_size( 'posts') ) AS table_size,
    pg_size_pretty( pg_relation_size( 'idx_posts_date' ) ) AS idx_date_size,
    pg_size_pretty( pg_relation_size( 'idx_posts_author' ) ) AS idx_author_size;

table_size | idx_date_size | idx_author_size
------------+---------------+----------------
59 MB      | 3600 kB       | 3600 kB

(1 row)
```

Both the indexes require the same space, but as we already said, we can drop the date-based index since it is no longer required. In fact, even with a specific date clause, the index is not used anymore:

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

Identifying unused indexes is important because it allows us to reclaim disk space and simplifies the management and data insertion: remember that every time the table changes, the index has to be updated, and this also requires extra resources, such as time and disk space.

Therefore, as you can see, it is really important to analyze the queries your applications execute the most and identify whether an index can help improve the execution speed, but also remember that an index has an extra cost in both space and maintenance, so don’t abuse the use of indexes.

But how can you identify unused indexes without even knowing about the ongoing queries?

Luckily, PostgreSQL provides you with detailed information about the usage of every index: the special pg_stat_user_indexes view provides information about how many times an index has been used and how. For example, to get information about the indexes over the posts table, you can execute something such as the following:

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

This tells us that idx_posts_date has been used 1 time, providing 500000 tuples, while idx_posts_author has been used 51 times and also provided far fewer tuples (only 8534), meaning it is very effective.

After observing this trend as time goes by, if the idx_posts_date is seldom used, you can safely drop it.

As a last example, let’s consider a poorly written query and the problem it implies: assume we want to extract all the authors that created posts on a certain date and that received, on the same date, a specific number of likes. A bad query could be the following:

```sql
SELECT u.username
FROM users u JOIN posts p ON p.author = u.pk WHERE p.created_on = CURRENT_DATE - 5
AND u.pk IN ( SELECT pp.author FROM posts pp WHERE likes = 5 and
p.created_on = created_on );
```

Clearly, there is no need for the subquery on the posts table, but this is a specific example to demonstrate why it is important to understand how to write good queries.

At a glance, the preceding query runs fast enough; it takes 190 milliseconds to execute, as demonstrated by EXPLAIN ANALYZE:

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

However, looking at the output, you can see that the Index Scan node using idx_posts_date on pp posts has a loops counter set to 1000: this means that this node is executed 1,000 times in the query. The node is the subquery, and that is how SQL can achieve the result: every time a tuple from the outer query is found, the inner query must be run and the results have to be joined. One possible option is to rewrite the query avoiding PostgreSQL having to do the looping, for example, by use of a subquery with a GROUP BY expression:

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
