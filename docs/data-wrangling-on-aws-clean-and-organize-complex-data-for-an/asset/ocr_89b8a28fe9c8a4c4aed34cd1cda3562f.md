```
`year` int)
PARTITIONED BY ( `product_category` string )
STORED AS PARQUET
LOCATION 's3://amazon-reviews-pds/parquet';
```

The following table compares the execution time, data scan, and cost between the TSV and Parquet tables:

| Query | Execution time | | | Data scan (GB) | | Cost ($5/TB scan) | |
| --- | --- | --- | --- | --- | --- | --- | --- |
| | TSV (amazon_reviews_tsv) | Parquet (amazon_reviews_parquet) | | TSV | Parquet | TSV | Parquet |
| SELECT count(*) FROM | 1 min 2.54 sec | 704 ms | | 32.22 GB | 0 | $0.15 | $0 |
| SELECT count(*) FROM "<table>" WHERE marketplace = 'US' | 1 min 25.66 sec | 1.298 sec | | 32.22 GB | 5.28 MB | $0.15 | $0.000025 |
| SELECT * FROM "<table>" WHERE marketplace = 'US' limit 1000; | 1.08 sec | 669 ms | | 1.88 MB | 2.69 MB | | |
| select distinct product_category from "<table_name>" | 1 min 16 sec | 1.38 sec | | 32.22 GB | 0 | | 0 |