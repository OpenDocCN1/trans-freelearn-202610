Here are the different **indexed** query plans and what they mean:

+   - **IXSCAN**: An index scan means we are traversing (usually a subset of) the B-tree in some order
+   - **IDHACK or EXPRESS_IXSCAN**: Special optimized index scan for _id indexes or other unique indexes
+   - **DISTINCT_SCAN**: Special index scan to support finding all distinct values in the B-tree
+   - **COUNT_SCAN**: Special index scan to support only getting counts from a subset of the B-tree

If there is no eligible index for the operation, then the query plan will be COLLSCAN, meaning a full collection scan.

> **Index preference over COLLSCAN**
> 
> If there is at least one eligible index for a query, then COLLSCAN will not be selected unless forced by the user.

Even though the efficiency of the query plan is roughly in the increasing order of this list, that’s not an absolute statement as it depends on the size of the B-tree subset that the scan is examining (a COUNT_SCAN of the entire index will likely be slower than an IXSCAN of a tiny portion of the index).

There are other *stages* or steps you will see in the query planner output. Here is what they mean:

+   - **FETCH**: Full document had to be read and, optionally, a filter applied to ensure it matches
+   - **SHARD_FILTER**: Usually applied last to ensure only documents owned by the shard are returned
+   - **SORT**: Indicates an in-memory sort of results
+   - **SORT_MERGE**: Used to merge multiple already-sorted streams of documents

There are a few other, more obscure stages, but we won’t worry about them for now.

> **Index-supported sorts omit SORT stage**
> 
> A sort that is supported by an index does *not* have any SORT stage in the plan as there is no extra step required to return sorted results.