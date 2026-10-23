Let’s take the last one as an example and see how it’s done internally:

```
world_temperatures=# explain analyze select extract (year from insert_time) as year, avg(temperature) avg_temp  from basilea group by 1 order by 2 desc limit 5;

QUERY PLAN

------------------------------------------------------------
------------------------------------------------------------
Limit  (cost=98293.21..98293.23 rows=5 width=64) (actual
time=380.284..380.286 rows=5 loops=1)
  ->  Sort  (cost=98293.21..99892.99 rows=639912 width=64) (actual
time=380.282..380.283 rows=5 loops=1)
        Sort Key: (avg(temperature)) DESC
        Sort Method: top-N heapsort  Memory: 25kB
      ->  HashAggregate  (cost=68067.20..87664.51 rows=639912 width=64)
(actual time=380.121..380.253 rows=73 loops=1)
        Group Key: EXTRACT(year FROM insert_time)
        Planned Partitions: 32  Batches: 1  Memory Usage: 817kB
      ->  Seq Scan on basilea  (cost=0.00..12074.90 rows=639912
width=40) (actual time=0.030..189.680 rows=639912 loops=1)
Planning Time: 0.170 ms
Execution Time: 380.480 ms
(10 rows)
```

As we can see, PostgreSQL performs a sequential scan on the whole table. Now consider the partitioned table, basilea_partitioned:

```
world_temperatures=# \d+ basilea_partitioned

Partitioned

table "public.basilea_partitioned"
    Column      |      Type       | [...] 
----------------+-----------------+--------
id              | integer         | [...] 
insert_time     | timestamp with time zone | [...]
```