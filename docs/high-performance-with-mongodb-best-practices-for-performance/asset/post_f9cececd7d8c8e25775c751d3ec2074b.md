
![](img/11c6cf3488b31bf4339aa4bd8ea9b45b_178_0.png)

## Pages in the Linux kernel versus WiredTiger storage engine

The concept of pages exists in both the Linux kernel and the WiredTiger storage engine. While the term *page* is used in both, it represents distinct concepts in each. A page in the Linux kernel is the fundamental unit of memory management.

For example, when trying to retrieve a document with a specific indexed field, such as _id, first an index B-tree is searched using the _id key to get the record ID value. Then, this record ID is used as the key to search the collection and return the value, which corresponds to a MongoDB document.

WiredTiger employs **multiversion concurrency control (MVCC)** to manage concurrent access to data in B-trees and pages. This allows multiple readers and writers to operate simultaneously, offering more concurrency than simpler schemes.

Data is persistently stored in files that contain the B-tree structures along with metadata. These files can grow dynamically as needed. On disk, you'll find a separate file for each index and MongoDB collection.

Another set of WiredTiger files is used to store the **journal**, which is often called the **write-ahead log (WAL)** in other database systems. Every operation that modifies the database is first written to the journal to ensure the durability of writes, should the server crash or lose power.

When a collection or index is first accessed, the root and first layer of internal nodes are loaded into a cache. As read and write operations occur, the in-memory cache is populated with pages on demand. By default, the WiredTiger cache uses 50% of the available memory minus 1 GB. For example, on a system with a total of 8 GB of RAM, the WiredTiger cache would use 3.5 GB of RAM (0.5 * (8 GB - 1 GB) = 3.5 GB).

The current implementation does not allow explicit pinning of documents or collections in the cache. The cache is shared across all databases and collections in a MongoDB server. WiredTiger periodically creates consistent checkpoints that capture a persistent snapshot of the database at a specific point in time.

WiredTiger supports fully ACID transactions, ensuring data consistency and integrity even in the face of failures. The following sections will tie all these concepts together by describing lookup, update, and insert operations.

## A lookup operation

When an application runs a query on the database, it is translated into one or more storage engine lookup operations, each of which does the following:

- 1. Assuming we’re reading a document identified by an indexed field, WiredTiger walks the index B-tree from root to the leaf containing the key.
- 2. If any pages are not in cache, they are loaded from storage into both the operating system filesystem cache and the WiredTiger cache. Eventually, WiredTiger finds the record ID corresponding to the indexed field.
- 3. The record ID from the previous step is used to walk the collection’s B-tree.
- 4. If any pages are not in cache, they are loaded from storage both into the operating system filesystem cache and the WiredTiger cache, where they are decrypted and uncompressed.
- 5. When WiredTiger finds the record ID, it returns the document from the page.

This operation ensures pages are used from memory if they are in the cache and avoids unnecessary storage I/O.

## An update operation

When the application updates a document, the database translates this into an update operation on the storage engine. Here’s what typically happens:

- 1. The document is read into cache as described in the previous section.
- 2. The page is marked as dirty, and the new values of the document are stored in an update list in memory.
- 3. If the write is part of a transaction, it waits until the transaction is committed.
- 4. An entry is written to the WiredTiger log that contains enough information to redo the operation should a crash occur. This log write is flushed to the storage device.
- 5. Later, when either a checkpoint runs or the page needs to be evicted from cache, WiredTiger walks through the page’s update list and writes the in-memory page to storage. Older versions of the document are written to a separate history store file.

This mechanism provides durability, crash recovery, and version tracking, all of which are essential for reliable database operations.

## An insert operation

For insert operations, WiredTiger performs the following:

- 1. It traverses the B-tree to find the appropriate leaf node to insert a key for the record/document.
- 2. When writing the node to storage, if it is full, it is split. The split operation may cascade back up the tree if the parent node is full.

Like reads and updates, inserts rely on efficient B-tree traversal and dynamic node management to maintain the performance and structural integrity of the data. All of the operations on the data involve traversing the B-tree data structures of the indexes and the collection data.

## Eviction, checkpointing, and recovery

The cache is limited in size, so WiredTiger needs to evict pages from the cache when it needs space for newer data. WiredTiger uses background threads to manage the eviction process. These threads continuously assess the cache state and perform eviction tasks, running alongside other database operations.

Pages are selected for eviction based on a variation of the least recently used algorithm, where the least recently accessed pages are candidates for eviction. This means frequently accessed pages should stay in memory/cache longer.

WiredTiger tracks three types of cache data: total data, dirty data, and updates. For each, it has two thresholds to manage eviction from the cache:

- **Target:** When the cache reaches this level, WiredTiger begins eviction activities to gradually bring the cache usage back down.
- **Trigger:** If this level is exceeded, WiredTiger enlists database operation threads to quickly free up space and prevent the cache from becoming full. This reduces the number of threads that can run simultaneous database operations.

These thresholds are configurable when running MongoDB self-managed. We’ll see examples later in the Configuration for improving performance section of this chapter. When using Atlas, MongoDB selects these thresholds based on the instance size you are running.

A checkpoint takes all of the in-memory data structures and writes any modifications to storage. You can configure how frequently checkpointing occurs. We’ll look at an example of this later in the chapter. The rest of the database server continues to read and write to pages during the checkpoint process.

The checkpoint process involves the following steps:

- 1. WiredTiger ensures all transactions that have been committed up to that point are durably stored in the disk image of the database by writing all the dirty pages from the cache.
- 2. All pending writes to files are flushed.
- 3. The journal files that contain the transaction logs are rotated.
- 4. WiredTiger marks the checkpoint end in its logs and metadata. This allows safe recovery from this point onward.

From a performance perspective, if a large volume of data is written between two checkpoints, many storage and file operations will take place during the second checkpoint. On IOPS- or bandwidth-constrained systems, this can result in periodic performance dips.

Recovery is required if the computer running the MongoDB server crashes or loses power without a proper shutdown. When the server restarts, it begins the recovery process by first loading the database state from the last completed checkpoint. If the previous shutdown was not clean, additional changes stored in the WiredTiger log (journal) are replayed on top of the last checkpoint. As a result, the longer the interval between checkpoints, the longer the recovery time will be.

## Compression and encryption

While compression helps save storage space, it comes at the cost of higher CPU usage. If your workload is already constrained by CPU usage on a server running MongoDB, you may choose to disable compression in either of the following cases:

- You are not concerned about the size of the files on your storage devices
- The data you are storing is not very compressible

Alternatively, you can switch to a different compression algorithm to reduce CPU usage, although this typically results in larger file sizes due to less effective compression. MongoDB currently supports three compression options:

| Compression algorithm | CPU usage | Compression efficiency | Notes |
|---|---|---|---|
| Snappy | Lowest | Lowest | Fastest option with minimal CPU cost; less effective compression |
| zstd | Medium | Better than Snappy | A balanced choice between speed and compression ratio |
| zlib | Highest | Best | Slowest, but offers the highest compression |

Table 7.1: Compression algorithms and their performance characteristics
