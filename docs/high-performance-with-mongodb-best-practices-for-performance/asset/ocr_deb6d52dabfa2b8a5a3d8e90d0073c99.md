# An insert operation

For insert operations, WiredTiger performs the following:

+   1. It traverses the B-tree to find the appropriate leaf node to insert a key for the record/document.
+   2. When writing the node to storage, if it is full, it is split. The split operation may cascade back up the tree if the parent node is full.

Like reads and updates, inserts rely on efficient B-tree traversal and dynamic node management to maintain the performance and structural integrity of the data. All of the operations on the data involve traversing the B-tree data structures of the indexes and the collection data.

# Eviction, checkpointing, and recovery

The cache is limited in size, so WiredTiger needs to evict pages from the cache when it needs space for newer data. WiredTiger uses background threads to manage the eviction process. These threads continuously assess the cache state and perform eviction tasks, running alongside other database operations.

Pages are selected for eviction based on a variation of the least recently used algorithm, where the least recently accessed pages are candidates for eviction. This means frequently accessed pages should stay in memory/cache longer.

WiredTiger tracks three types of cache data: total data, dirty data, and updates. For each, it has two thresholds to manage eviction from the cache:

+   - **Target:** When the cache reaches this level, WiredTiger begins eviction activities to gradually bring the cache usage back down.
+   - **Trigger:** If this level is exceeded, WiredTiger enlists database operation threads to quickly free up space and prevent the cache from becoming full. This reduces the number of threads that can run simultaneous database operations.

These thresholds are configurable when running MongoDB self-managed. We’ll see examples later in the Configuration for improving performance section of this chapter. When using Atlas, MongoDB selects these thresholds based on the instance size you are running.

A checkpoint takes all of the in-memory data structures and writes any modifications to storage. You can configure how frequently checkpointing occurs. We’ll look at an example of this later in the chapter. The rest of the database server continues to read and write to pages during the checkpoint process.