

The next figure shows a few data pages loading into the shared buffers’ memory location:

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_426_0.png)

When a transaction modifies some data, it does so by modifying the in-copy memory, which means it modifies the “shared buffers” area.

At this point, the in-memory copy of the data does not correspond to the stored version, and it is here that PostgreSQL has to guarantee consistency and persistency without losing performance.

What happens is that the data is kept in memory but is marked as dirty, meaning that it is a copy not yet synchronized with the on-disk original source. Once the changes to a dirty buffer have been committed, PostgreSQL consolidates the changes in the WALs and keeps the dirty buffer in memory, to be served as the most recent available copy for other transactions and connections.

Why is the WAL space considered to be more efficient than overwriting the original data block in the $PGDATA/base directory? The trick is that in order to find the exact position on the disk storage where the block has to be overwritten, PostgreSQL should have to perform what is called a random-seek, which is a costly I/O operation. On the other hand, WALs are sequentially written as a journal, and therefore, there is no need to perform a random-seek. Writing the WALs prevents the I/O performance degradation and allows PostgreSQL to overwrite the data block in the future, when, for instance, the cluster is not overloaded and has I/O bandwidth available.

Every time a transaction performs a COMMIT, its actions and modified data are permanently stored in a piece of the WAL, in particular a specific part of the current WAL segment (more on this later). Therefore, PostgreSQL can reproduce the transaction and its effects in order to perform the very same data changes.

This, however, does not suffice in making PostgreSQL reliable: PostgreSQL makes a big effort to ensure data actually hits the disk storage. In particular, during the writing of the WALs, PostgreSQL isolates itself from the outside world, disabling operating system signals, so that it cannot be interrupted. Moreover, PostgreSQL issues fsync(2), a particular operating system call that forces the filesystem cache to flush data on disk.

PostgreSQL does all of this in order to ensure that the data physically hits the disk layer, but it must be clear that if the filesystem, or the disk controller (that is, the hardware), lies, the data could not be physically on the disk. This is important, but PostgreSQL cannot do anything about that and has to trust what the operating system (and, thus, the hardware) reports back as feedback.

In any case, COMMIT will return success to the invoking transaction if, and only if, PostgreSQL has been able to write the changes on the disk. Therefore, at the transaction level, if a COMMIT succeeds (that is, there is no error), the data has been written in the WALs and, therefore, can be assumed to be safe on the storage layer.

WALs are split into chunks called segments. A segment is a file made of exactly 16 MB of changes in data. While it is possible to modify the size of segments during initdb, we strongly discourage this and will assume every segment is 16 MB in size.

This means that PostgreSQL writes, sequentially, a single file at a time (that is, a WAL segment), and when this has reached the size of 16 MB, it is closed and a new 16 MB file is created. The WAL segments are stored in the pg_wal directory under $PGDATA. Every segment has a name made up of hexadecimal digits, 24 characters long.

The first eight characters indicate the so-called *timeline* of the cluster (something related to physical backups and replication), the next eight digits indicate an increasing sequence number named the **Log Sequence Number (LSN)**, and the last eight digits provide the offset within the LSN. Here’s an example:

```
$ ls -l $PGDATA/pg_wal
0000000700000247000000A8
0000000700000247000000A9
0000000700000247000000AA
0000000700000247000000AB
0000000700000247000000AC
0000000700000247000000AD
...
```

In the previous content of the pg_wal, you can see that every WAL segment has the same timeline, number 7, and the LSN is 247. Every file, then, has a different offset, with the first one being A8, the second A9, and so on. As you can imagine, WAL segment names are not made for humans, but PostgreSQL knows exactly how and in which file it has to search for information.

Sooner or later, depending on the memory resources and usage of the cluster, the data in memory will be written back to its original disk positions, meaning that the WALs serve only as temporary safe storage on disk. The reason for that is not only tied to a performance boost, as already explained, but also to allow data restoration in the event of a crash.

# WALs as a rescue method in the event of a crash

When you cleanly stop a running cluster, for example, by means of pg_ctl, PostgreSQL ensures that all dirty data in memory is flushed to the storage in the correct order, and then halts itself.

But what happens if the cluster is uncleanly stopped, for example, by means of a power failure?

This event is named a **crash**, and once PostgreSQL starts over, it performs a so-called **crash recovery**. In particular, PostgreSQL understands it has stopped in an unclean way, and therefore, the data on the storage might not be the last version that existed in memory when the cluster terminated its activity. But PostgreSQL knows that all committed data is at least present in the WALs and, therefore, starts reading the WALs in what is called **WAL replay**, adjusting the data in the storage according to what is in the WALs. Until the crash recovery has completed, the cluster is not usable and does not accept connections; once the crash recovery has finished, the cluster knows that the data on the storage has been made coherent, and therefore, normal operativity can start again.

This process allows the cluster to somehow self-heal after an external event that caused its normal life cycle to abort. This makes it clear that the main aim of the WALs is not to avoid performance degradations but, rather, to ensure the cluster is able to recover after a crash. And in order to be able to do that, it must have data written permanently to the storage, but thanks to the sequential way in which WALs are written, data is made persistent with less I/O penalties.

# Checkpoints

Sooner or later, the cluster must make every change that has already been written in WALs also available in the data files; that is, it has to write tuples in an I/O-scattered way. These writes happen at very specific times, named **checkpoints**. A checkpoint is a point in time at which a database makes an extra effort to ensure that everything already present in the WALs is also written in the correct position in the data storage.

The following diagram helps to understand what happens during a CHECKPOINT:

![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_430_0.png)

*Figure 11.4: An example of a CHECKPOINT*

In fact, when a checkpoint is reached, the database must force every dirty buffer from memory to disk, and this usually means that an I/O spike is introduced; during such a spike, other concurrent database activities, such as getting new data from the storage, will be penalized because the I/O bandwidth is temporarily exhausted from the checkpoint activity.

For the preceding reasons, it is very important to carefully tune checkpoints, and in particular, their tuning must reflect the cluster workload.

Checkpoints can be tuned by means of three main configuration parameters that interact with each other, which are explained in the following subsections.

# checkpoint_timeout and max_wal_size

Checkpoint frequency can be tuned by two orthogonal parameters: `max_wal_size` and `checkpoint_timeout`.

The `max_wal_size` parameter dictates how much space the pg_wal directory can occupy. Since at every checkpoint WAL segments are recycled, the pg_wal directory tends to occupy the very same size eventually. Tuning the `max_wal_size` parameter specifies after how many data changes the checkpoint must be completed, and therefore, this parameter is a *quantity* specification.

`checkpoint_timeout` expresses after how much time the checkpoint must be forced.

The two parameters are orthogonal, meaning that the first that happens triggers the checkpoint execution; your database produces data changes over the `max_wal_size` parameter or when the `checkpoint_timeout` time has elapsed.

As an example, let’s take a system with the default settings:

```
```text
forumdb=> SHOW checkpoint_timeout;
checkpoint_timeout
------------------
5min
(1 row)

forumdb=> SHOW max_wal_size ;
max_wal_size
------------
1GB
(1 row)
```
```
