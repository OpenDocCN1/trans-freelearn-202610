```
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

That is exactly the output a normal EXPLAIN command would have produced for the same query.

The beauty of this approach is that you don’t have to worry about or remember to execute EXPLAIN on queries or collected queries; you simply have to inspect the logs to find out the execution plan of slow queries. Once you have fixed queries such as the preceding, by creating indexes, for example, you can raise the threshold of auto-explain to catch slower queries and iterate the process again.

In order to demonstrate the difference in the output of running the query with auto_explain.log_analyze turned on, the following is the output produced for the very same query:

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