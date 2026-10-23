```
tup_inserted | 1391
tup_updated | 46
tup_deleted | 0
stats_reset |
```

As you can see, the forumdb database doesn’t have any conflicts or deadlocks, and the number of committed transactions (the xact_commit column) is much higher than the number of aborted transactions (the xact_rollback column). Therefore, we can assume that the database is fine and that the applications are issuing good queries.

The last column, stats_reset, is particularly important since it indicates whenever the statistics information for a database has been reset, meaning deleted. Knowing how much time has elapsed since the statistics have been reset helps in validating the database. If the column is empty, the database statistics have never been reset manually.

# Inspecting tables and indexes

The pg_stat_user_tables and pg_stat_user_indexes special catalogs provide detailed information about the usage of a table or an index, such as the number of tuples, the number of reads and writes, and so on.

In order to better understand, consider a table where some transactions have been deleted or updated, and new records inserted; the following query provides detailed information about the status of that table:

```
forumdb=# SELECT relname, seq_scan, idx_scan,
    n_tup_ins, n_tup_del, n_tup_upd, n_tup_hot_upd,
    n_live_tup, n_dead_tup,
    last_vacuum, last_autovacuum,
    last_analyze, last_autoanalyze
    FROM pg_stat_user_tables;

-[ RECORD 1 ]---------------------------------------------
relname | tags
seq_scan | 20
idx_scan | 0
n_tup_ins | 100007
n_tup_del | 63
```