The planner has chosen to fire up the parallel query execution: the top node is a Gather node, and thus acts as a synchronization point for parallel workers. In particular, the planner has decided to use two parallel processes to complete the query. The rightmost node, and therefore the first one being executed, is an Index Scan node on the users table. Results are hashed and, in the meantime, a Parallel Seq Scan is fired to extract data from the posts table. The results are joined and the Gather collects the final result set. The total execution time of this query can be easily obtained with EXPLAIN ANALYZE, which shows us that the query takes around 140 milliseconds to complete:

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