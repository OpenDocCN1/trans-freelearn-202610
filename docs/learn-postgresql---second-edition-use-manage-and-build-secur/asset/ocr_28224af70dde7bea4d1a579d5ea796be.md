If you need to parse the EXPLAIN output with an application or tool, you should stick to one of the structured formats, not the default text one.

# EXPLAIN ANALYZE

The ANALYZE mode of EXPLAIN enhances the command by effectively running the query to explain. Therefore, the command does a double task: it prints out the best plan to execute the query and it runs the query, also reporting back some statistical information.

To better understand the concept, consider the output of EXPLAIN ANALYZE compared to the output of a plain EXPLAIN command:

```
forumdb=> EXPLAIN SELECT * FROM posts;
                    QUERY PLAN
--------------------------------------------------
Seq Scan on posts  (cost=0.00..31.04 rows=1004 width=71)
(1 row)

forumdb=> EXPLAIN ANALYZE SELECT * FROM posts;
                    QUERY PLAN
--------------------------------------------------
--------------------------------------------------
Seq Scan on posts  (cost=0.00..31.04 rows=1004 width=71) (actual time=0.006..0.101 rows=1004 loops=1)
Planning Time: 0.052 ms
Execution Time: 0.163 ms
(3 rows)
```

The output of the EXPLAIN ANALYZE command is enhanced by the “actual” part of every node: the executor reports back how the execution of the node went exactly. Therefore, while EXPLAIN can only estimate the costs of a node, the EXPLAIN ANALYZE provides feedback on the execution time (expressed in milliseconds), the effective number of rows, and how many times a node has been executed (loops).

The node time is expressed, similarly to the cost, in a startup time and a final time, which is the time taken for the node to complete its execution. Therefore, in the preceding example, PostgreSQL took 0.006 milliseconds to “warm up” and completed the query execution in 0.101 milliseconds, so the node required 0.107 milliseconds to complete its job.