The wal_level setting can be set to minimal, which is a single cluster with all the information to survive a crash, or replica (the default), which makes the WAL also useful for physical replication scenarios, and finally, it can be turned to logical, which makes the WALs contain information also for logical replication scenarios.

wal_sync_method allows the administrator to configure a specific operating system call to sync dirty buffers. All the POSIX operating systems implement fsync(2), but some of them provide special flavors that behave faster or better depending on the filesystem. It is possible to specify the exact name of the system call to use via wal_sync_method. Usually, PostgreSQL is shipped with an appropriate configuration for the operating system.

But how can you discover the best (or just the available) fsync(2) implementation that fits your operating system? You can launch the pg_test_fsync program on your machine to get a good guess about the possible methods you can use, as well as the best one. As an example, on a FreeBSD machine, the program provides the following output:

```
$ pg_test_fsync
5 seconds per test

O_DIRECT supported on this platform for open_datasync and open_sync.

Compare file sync methods using one 8kB write:
(in wal_sync_method preference order, except fdatasync is Linux's default)
    open_datasync                         n/a
    fdatasync                     6845.727 ops/sec    146 usecs/op
    fsync                         3685.769 ops/sec    271 usecs/op
    fsync_writethrough                    n/a
    open_sync                     2521.228 ops/sec    397 usecs/op
...
```

You should compare the available options and choose the fastest one. So, in the preceding example, wal_sync_method = open_datasync is the best choice.

synchronous_commit is a multiple-choice option that indicates how many WALs have to be written to the disk before returning a “success” state to the transaction issuing the COMMIT. By default, this setting has the value on, indicating that every single bit must hit the disk before the transaction is allowed to succeed.