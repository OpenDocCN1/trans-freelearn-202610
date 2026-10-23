

-----+-----------+---------------+-------
 1 | 2022-01-01 | Operating Systems |   0
 2 | 2022-02-01 | Linux           |   1
(2 rows)

forumdb=> select * from tablespace_part_tags_date_2023 ;
pk | ins_date | tag | level
-----+-----------+---------------+-------
 3 | 2023-03-01 | BSD Unix |   1
(1 row)

forumdb=>select * from tablespace_part_tags_date_default;
pk | ins_date | tag | level
-----+-----------+---------------+-------
 4 | 2018-04-01 | Rocky Linux Distro |   2
(1 row)

正如我们在本练习中所看到的，数据已被拆分到不同的表空间中，因此我们将速度翻倍。这是一种非常有效的技术。

## 一个简单的案例研究

在本节的最后，我们将不使用 forumdb 数据库。我们将使用的是名为 world_temperatures 的数据库，其公开数据已从 https://www.meteoblue.com/it/tempo/archive/export 上的公开 CSV 导入。

db-world-temperatures 数据库备份可以在 packtpub GitHub 的第 9 章目录中找到，文件名为 backup-db-world-temperatures.sql.gz。如果你使用的是 Docker 镜像，你将已经拥有所有可用内容；否则，要导入数据库，请在你的服务器上运行 PostgreSQL：

```
$ gunzip < backup-db-world-temperatures.sql.gz | psql
```

如果你使用的是 Docker 镜像，只需执行以下操作：

```
postgres@learn_postgresql:~$ psql -U postgres world_temperatures
```

现在，db-world-temperatures 数据库已准备就绪。在数据库中，你会找到一个未分区的表 basilea 和一个分区的表 basilea_partitioned；这两个表都包含从 1950 年到 2022 年对巴塞尔市的温度信息，按规则的每小时间隔采样。现在，让我们看看在搜索分区表和非分区表之间的行为差异。在继续练习之前，如果你还不熟悉 EXPLAIN 语句的行为，请查看第 13 章《EXPLAIN 语句》。

让我们先使用非分区表，并编写一个查询，其结果返回从 1950 年开始的期间内 5 个最冷年份的平均温度：

```
world_temperatures=# select extract (year from insert_time) as year,
avg(temperature) avg_temp  from basilea group by 1 order by 2  limit 5;
year |     avg_temp
--------+---------------
1956 | 8.8073832344034608
1963 | 8.9077977708904110
1980 | 9.3459840948315118
1969 | 9.3705488990867580
1972 | 9.3749401615437158
(5 rows)
```

同样，让我们获取 5 个最暖年份：

```
world_temperatures=# select extract (year from insert_time) as year,
avg(temperature) avg_temp  from basilea group by 1 order by 2 desc limit 5;
year |     avg_temp
--------+---------------
2022 | 12.7320820592465753
2018 | 12.5638742964611872
2020 | 12.3662106902322404
2014 | 12.0601722329908676
2015 | 11.9246379973744292
(5 rows)
```

让我们以最后一个为例，看看其内部是如何完成的：

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

正如我们所见，PostgreSQL 对整个表执行了顺序扫描。现在考虑分区表 basilea_partitioned：

```
world_temperatures=# \d+ basilea_partitioned

Partitioned

table "public.basilea_partitioned"
    Column      |      Type       | [...] 
----------------+-----------------+--------
id              | integer         | [...] 
insert_time     | timestamp with time zone | [...]
```

```
temperature | numeric(8,6)             | [...]

Partition key: RANGE (insert_time)
Indexes:
    "basilea_partitioned_pkey" PRIMARY KEY, btree (id, insert_time)
Partitions: basilea_partitioned_1950 FOR VALUES FROM ('1949-12-31
23:00:00+00') TO ('1950-12-31 23:00:00+00'),
            basilea_partitioned_1951 FOR VALUES FROM ('1950-12-31
23:00:00+00') TO ('1951-12-31 23:00:00+00'),
[...]
            basilea_partitioned_2023 FOR VALUES FROM ('2022-12-31
23:00:00+00') TO ('2023-12-31 23:00:00+00'),
            basilea_partitioned_default DEFAULT
```

该表按年份从 1950 年到 2022 年进行分区，并且还有一个默认表（如“默认分区”部分所述）。

数据在所有子表之间平均分配，如果我们尝试执行相同的查询，我们将得到以下结果：

```sql
world_temperatures=# explain analyze select extract (year from insert_time) as year, avg(temperature) avg_temp  from basilea_partitioned group by 1 order by 2 desc limit 5;

QUERY PLAN

------------------------------------------------------------------------
------------------------------------------------------------------------
--------------------------------------------
 Limit  (cost=13183.59..13183.61 rows=5 width=64) (actual time=169.996..174.092 rows=5 loops=1)
   ->  Sort  (cost=13183.59..13184.09 rows=200 width=64) (actual time=169.995..174.090 rows=5 loops=1)
         Sort Key: (avg(basilea_partitioned.temperature)) DESC
         Sort Method: top-N heapsort  Memory: 25kB
         ->  Finalize GroupAggregate  (cost=13127.60..13180.27 rows=200 width=64) (actual time=169.812..174.060 rows=73 loops=1)
               Group Key: (EXTRACT(year FROM basilea_partitioned.insert_time))
```

```
-> Gather Merge  (cost=13127.60..13174.27 rows=400
width=64) (actual time=169.802..173.941 rows=132 loops=1)
        Workers Planned: 2
        Workers Launched: 2
        -> Sort  (cost=12127.58..12128.08 rows=200 width=64)
(actual time=140.386..140.401 rows=44 loops=3)
                Sort Key: (EXTRACT(year FROM basilea_
partitioned.insert_time))
                Sort Method: quicksort  Memory: 30kB
                Worker 0:  Sort Method: quicksort  Memory: 29kB
                Worker 1:  Sort Method: quicksort  Memory: 29kB
        -> Partial HashAggregate
(cost=12116.93..12119.93 rows=200 width=64) (actual time=140.330..140.363
rows=44 loops=3)
                Group Key: (EXTRACT(year FROM basilea_
partitioned.insert_time))
                Batches: 1  Memory Usage: 64kB
                Worker 0:  Batches: 1  Memory Usage: 48kB
                Worker 1:  Batches: 1  Memory Usage: 64kB
        -> Parallel Append  (cost=0.00..10780.64
rows=267259 width=40) (actual time=0.010..79.251 rows=213304 loops=3)
        -> Parallel Seq Scan on basilea_
partitioned_1952 basilea_partitioned_3  (cost=0.00..129.59 rows=5167
width=40) (actual time=0.010..3.176 rows=8784 loops=1)
[...]
        -> Parallel Seq Scan on basilea_
partitioned_2018 basilea_partitioned_69  (cost=0.00..128.41 rows=5153
width=40) (actual time=0.004..2.671 rows=8760 loops=1)
        -> Parallel Seq Scan on basilea_
partitioned_2021 basilea_partitioned_72  (cost=0.00..128.41 rows=5153
width=40) (actual time=0.003..2.651 rows=8760 loops=1)
        -> Parallel Seq Scan on basilea_
partitioned_default basilea_partitioned_75  (cost=0.00..21.10 rows=888
width=40) (actual time=0.000..0.000 rows=0 loops=1)
        -> Parallel Seq Scan on basilea_
partitioned_2023 basilea_partitioned_74  (cost=0.00..1.01 rows=1 width=40)
(actual time=0.008..0.009 rows=1 loops=1)
Planning Time: 0.698 ms
```

