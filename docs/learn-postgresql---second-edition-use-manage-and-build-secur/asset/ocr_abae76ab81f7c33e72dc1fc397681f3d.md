Such values are shared with manual VACUUM:

```
forumdb=> SELECT name, setting  FROM pg_settings
    WHERE name like 'vacuum_cost%';
            name            | setting
----------------------------+---------
vacuum_cost_delay          | 0
vacuum_cost_limit          | 200
vacuum_cost_page_dirty     | 20
vacuum_cost_page_hit       | 1
vacuum_cost_page_miss      | 2
(5 rows)
```

Such values are used for both manual VACUUM and autovacuum, with the exception that autovacuum has its own autovacuum_vacuum_cost_limit, which is usually set to 200. On the other hand, manual VACUUM has vacuum_cost_delay set to 0, meaning essentially that a manual VACUUM process will never suspend itself. After all, the database administrator wants the manual VACUUM to finish as soon as possible.

Similar parameters exist for the ANALYZE part because the autovacuum background process performs VACUUM ANALYZE, and therefore, you have autovacuum_analyze_threshold and autovacuum_analyze_scale_factor, which are in charge of defining the window of activity for the ANALYZE part (which is involved in updating the statistics on the contents of the table).

# Summary

PostgreSQL exploits MVCC to provide high concurrent access to underlying data, and this means that every transaction perceives a snapshot of the data while the system keeps different versions of the same tuples. Sooner or later, invalid tuples will be removed, and storage space will be reclaimed. On one hand, MVCC provides better concurrency, but on the other hand, it requires extra effort to reclaim the storage space once transactions no longer reference dead tuples. PostgreSQL provides VACUUM for this aim and also has a background process machinery, named autovacuum, to periodically and non-invasively keep a system clean and healthy.

In order to improve I/O and reliability, PostgreSQL stores data in a journal written sequentially, the WAL. The WAL is split into segments, and at particular time intervals, named checkpoints, all the dirty data in memory is forced to a specified position in the storage, and the WAL segments are recycled.