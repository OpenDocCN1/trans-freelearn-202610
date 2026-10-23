```
Execution Time: 174.250 ms
(97 rows)
```

As we can see, PostgreSQL first performs a parallel sequential scan and then a parallel append to merge all the data that has been taken from the child tables.

Now let’s try to perform the same operation, but filtering for the years ranging from 2021 to 2022. On the non-partitioned table we will have the following:

```
world_temperatures=# explain analyze select extract (year from insert_time) as year, avg(temperature) avg_temp from basilea where insert_time >= '2021-01-01' and insert_time < '2023-01-01' group by 1 order by 2 desc limit 5;

QUERY PLAN

---------------------------------------------------------------
 Limit  (cost=11498.05..11498.06 rows=5 width=64) (actual time=24.532..28.875 rows=2 loops=1)
   ->  Sort  (cost=11498.05..11544.43 rows=18554 width=64) (actual time=24.529..28.871 rows=2 loops=1)
         Sort Key: (avg(temperature)) DESC
         Sort Method: quicksort  Memory: 25kB
       ->  Finalize HashAggregate  (cost=10911.56..11189.87 rows=18554 width=64) (actual time=24.467..28.859 rows=2 loops=1)
             Group Key: (EXTRACT(year FROM insert_time))
             Batches: 1  Memory Usage: 793kB
           ->  Gather  (cost=9133.43..10795.60 rows=15462 width=64) (actual time=24.159..28.707 rows=6 loops=1)
                 Workers Planned: 2
                 Workers Launched: 2
               ->  Partial HashAggregate  (cost=8133.43..8249.40 rows=7731 width=64) (actual time=20.170..20.211 rows=2 loops=3)
                     Group Key: EXTRACT(year FROM insert_time)
                     Batches: 1  Memory Usage: 409kB
                     Worker 0:  Batches: 1  Memory Usage: 409kB
                     Worker 1:  Batches: 1  Memory Usage: 409kB
                   ->  Parallel Seq Scan on basilea (cost=0.00..8094.78 rows=7731 width=40) (actual time=16.211..18.330 rows=5840 loops=3)
```