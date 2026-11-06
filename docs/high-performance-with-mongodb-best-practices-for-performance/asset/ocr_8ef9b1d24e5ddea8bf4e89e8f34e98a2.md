The checkpoint process involves the following steps:

+   1. WiredTiger ensures all transactions that have been committed up to that point are durably stored in the disk image of the database by writing all the dirty pages from the cache.
+   2. All pending writes to files are flushed.
+   3. The journal files that contain the transaction logs are rotated.
+   4. WiredTiger marks the checkpoint end in its logs and metadata. This allows safe recovery from this point onward.

From a performance perspective, if a large volume of data is written between two checkpoints, many storage and file operations will take place during the second checkpoint. On IOPS- or bandwidth-constrained systems, this can result in periodic performance dips.

Recovery is required if the computer running the MongoDB server crashes or loses power without a proper shutdown. When the server restarts, it begins the recovery process by first loading the database state from the last completed checkpoint. If the previous shutdown was not clean, additional changes stored in the WiredTiger log (journal) are replayed on top of the last checkpoint. As a result, the longer the interval between checkpoints, the longer the recovery time will be.

# Compression and encryption

While compression helps save storage space, it comes at the cost of higher CPU usage. If your workload is already constrained by CPU usage on a server running MongoDB, you may choose to disable compression in either of the following cases:

+   - You are not concerned about the size of the files on your storage devices
+   - The data you are storing is not very compressible

Alternatively, you can switch to a different compression algorithm to reduce CPU usage, although this typically results in larger file sizes due to less effective compression. MongoDB currently supports three compression options:

| Compression algorithm | CPU usage | Compression efficiency | Notes |
|---|---|---|---|
| Snappy | Lowest | Lowest | Fastest option with minimal CPU cost; less effective compression |
| zstd | Medium | Better than Snappy | A balanced choice between speed and compression ratio |
| zlib | Highest | Best | Slowest, but offers the highest compression |

Table 7.1: Compression algorithms and their performance characteristics