For example, note in the following EXPLAIN statement how the actual time is missing from the output:

```
forumdb=> EXPLAIN (ANALYZE on, TIMING off) SELECT * FROM categories;
QUERY PLAN
------------------------------------------------------------
--------
Seq Scan on categories  (cost=0.00..1.05 rows=5 width=68) (actual rows=5 loops=1)
Planning Time: 0.113 ms
Execution Time: 0.047 ms
(3 rows)
```

The SUMMARY option reports the total time spent in planning for the execution and the time spent for the query execution so that you can get an idea of how much effort the planner has used to find out the best execution plan.

The BUFFERS option, which defaults to off, provides information about the data buffers the query used to complete. For example, note how there is buffer-related information on the Execution node in the following query:

```
forumdb=> EXPLAIN (ANALYZE, BUFFERS on) SELECT * FROM posts;
QUERY PLAN
------------------------------------------------------------
----------------
Seq Scan on posts  (cost=0.00..31.04 rows=1004 width=71) (actual time=0.006..0.100 rows=1004 loops=1)
  Buffers: shared hit=21
Planning Time: 0.061 ms
Execution Time: 0.171 ms
(4 rows)
```

The buffer information is not trivial to analyze and can be split into two parts: a prefix and a suffix.

The prefix can be any of the following:

+   - shared, meaning a PostgreSQL shared buffer, which is the database in-memory cache
+   - temp, meaning temporary memory (used for sorting, hashing, and so on)
+   - local, meaning temporary database objects space (for instance, temporary tables)