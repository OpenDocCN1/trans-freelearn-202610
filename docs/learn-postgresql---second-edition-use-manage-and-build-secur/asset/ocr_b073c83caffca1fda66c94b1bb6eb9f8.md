```
Filter: ((insert_time >= '2021-01-01 00:00:00+00'::timestamp with time zone) AND (insert_time < '2023-01-01 00:00:00+00'::timestamp with time zone))
Rows Removed by Filter: 207464
Planning Time: 0.248 ms
Execution Time: 30.043 ms
(20 rows)
```

And for the partitioned table, we will have the following:

```
world_temperatures=# explain analyze select extract (year from insert_time) as year, avg(temperature) avg_temp  from basilea_partitioned where insert_time >='2021-01-01' and insert_time < '2023-01-01' group by 1 order by 2 desc limit 5;

QUERY PLAN

-----------------------------------------
Limit  (cost=618.10..618.11 rows=5 width=64) (actual time=15.205..15.208 rows=2 loops=1)
  ->  Sort  (cost=618.10..618.60 rows=200 width=64) (actual time=15.203..15.205 rows=2 loops=1)
        Sort Key: (avg(basilea_partitioned.temperature)) DESC
        Sort Method: quicksort  Memory: 25kB
      ->  HashAggregate  (cost=611.78..614.78 rows=200 width=64)
(actual time=15.190..15.194 rows=2 loops=1)
        Group Key: (EXTRACT(year FROM basilea_partitioned.insert_time))
        Batches: 1  Memory Usage: 40kB
          ->  Append  (cost=0.00..524.19 rows=17517 width=40) (actual time=0.032..8.804 rows=17520 loops=1)
              ->  Seq Scan on basilea_partitioned_2021
basilea_partitioned_1  (cost=0.00..217.30 rows=8758 width=40) (actual time=0.030..4.466 rows=8759 loops=1)
        Filter: ((insert_time >= '2021-01-01 00:00:00+00'::timestamp with time zone) AND (insert_time < '2023-01-01 00:00:00+00'::timestamp with time zone))
Rows Removed by Filter: 1
```