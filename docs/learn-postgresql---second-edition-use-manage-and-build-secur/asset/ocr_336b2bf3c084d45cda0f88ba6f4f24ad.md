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