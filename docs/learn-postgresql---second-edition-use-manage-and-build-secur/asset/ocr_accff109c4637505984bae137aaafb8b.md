The suffix can be any of the following:

+   - hit, providing the number of memory successes
+   - read, providing the number of buffers read from the storage (therefore, not in the cache)
+   - dirtied, the number of buffers modified by the query
+   - written, the number of buffers removed from the PostgreSQL cache and written to disk
+   - lossy, the number of buffers that PostgreSQL has checked in memory in a second pass

Combining the prefix and the suffix provides information on the buffers. For example, in the previous query, the buffer line contained shared hit=21, which reads as “21 buffers have been successfully found in the database cache, no more operations on buffers are required.”

The WAL option provides information about the WAL usage of a writing statement. As an example, consider the following query, which adds a bunch of fake usernames to the users table:

```
forumdb=> EXPLAIN (ANALYZE on, WAL on, FORMAT yaml)
INSERT INTO posts( title, content, author, created_on, category )
SELECT 'A random post title ' || v, md5( v::text ), v%2 + 1, current_date - v, v%5 + 1
FROM generate_series( 1, 100000 ) v;

QUERY PLAN
--------------------------------------------------
- Plan:
    + 
    Node Type: "ModifyTable"
    +
...
    Actual Loops: 1
    +
    WAL Records: 506908
    +
    WAL FPI: 651
    +
    WAL Bytes: 46291062
    +
...
```

As you can see, the output reports information about the number of WAL records that have been generated (506908), the number of WAL Full Page Images (FPIs) (651), and the number of bytes written into the WAL logs (46291062). Therefore, the above statement has generated around 46 MB of WAL traffic.