
The limitations of the result-set caching are as follows:

- The RecordViewCache is not shared between Dynamics AX clients.
- The RecordViewCache is deactivated as soon as the RecordViewCache object goes out of scope or is destroyed.

In some cases result-set caching may be dangerous and hence only careful use is recommended. The following facts about result-set caching will be helpful in deciding the use of result-set caching.

- The database is simultaneously updated with the RecordViewCache.
- Updating the key of a row that did not qualify the result set at the time of instantiation will not result in the row being included in the cache.
- Inserts are always included in the RecordViewCaches for that table. It is definitely an advantage; however, care should be taken when inserting a large number of rows in a table and at the same time having a RecordViewCache on the same table as it will prolong the cache update time.
- A `delete` will remove the row from RecordViewCaches on that table but not the table
- A `Delete_from` will invalidate RecordViewCaches on that table.

As mentioned above the RecordViewCache can go out of synchronization and hence we may need to re-synchronize it again. X++ has a method `reread`, which retrieves the data from database and updates the cached copy.

## Indexing

Index design is very critical as far as database performance optimization is concerned. In general indexes are stored in a separate file or table to that of the data table. Thus searches are performed through a small set of columns (indexed columns only) and then the data is ultimately retrieved from the table itself by the database using the index. The objectives are as follows.

Primary keys or indexes are generally used to uniquely identify a record. The original intention of primary and foreign keys was that of the representation and enforcement of referential integrity between related tables. Primary keys are now used as the most important index for a table and do not necessarily have referring foreign keys in other tables. Primary keys are usually attached to the data space of the table itself but not always. This is database product specific.

Non-primary indexes are indexes constructed from one or more table columns. Non-primary indexes may or may not include the primary index column or columns. The purpose of non-primary indexes is to improve database access performance. These indexes will be created as a separate file within the database. Thus when searching these indexes a small number of columns is loaded into memory for searching. These indexes have virtual memory addresses into the data space of the table, thus allowing rapid access between index space and table space.

Note that the memory or cache space required for an index is much smaller than that of a data table since the row length of an index table is smaller. The reason behind this is that indexes will have a fewer columns in comparison to the data table. Thus in the case of page swapping in and out of memory or cache and disk, indexes load many more pages into memory at once since they are smaller than data tables and thus indexes can be traversed much more rapidly than data table spaces. Different databases handle this in different ways.

There are different types of indexes available.

Indexes can be unique or non-unique. Unique indexes are generally used as primary keys (not always) and non-unique indexes contain duplications that are generally used for database access.

Clustered indexes are generally data table space forms of hashing or btree algorithms. A clustered index clusters or groups the actual data table rows of the table where the actual data rows reside in the leaf pages of the index.

A non-clustered index sets the leaf pages of the index as pointers to the data pages containing the rows in the table.

Any type of indexes can be ordered as ascending or descending. Ascending indexes are a usual practice.

Now it is obvious that indexing does not make every database operation fast; operations other than read operations are even slowed because of indexing due to the increased efforts required to update index files in addition to the table.

## What to Index

To unleash the maximum benefits from indexing while avoiding or minimizing the disadvantages of indexing, what to index or what not to index should be carefully decided. The following are rules of thumb:

Use indexes where frequent queries are performed:

- Columns are used in joins.
- Columns are used in where clauses or groupby.
- Columns are used in order-by clauses.
- The group-by clause can be enhanced by indexing when the range of values being grouped is small in relation to the number of rows in the table selected.

Indexes will degrade performance of inserts and updates, sometimes substantially. Following are a few such cases:

- Tables with a small number of rows
- Static tables
- Columns with a wide range of values
- Tables changed frequently and with a low amount of data access
- Columns not used in data access query `select` statements
- Tables having no fixed width or numeric data column to index or indexing not done on that column

## Types of Indexes

There are several types of indexes and each has its own merits and demerits. Clustered indexes are indexes made in the table itself and not in a separate index file and hence with this type of indexes the entire table is sorted as per the indexed column. This is the primary difference between clustered and non-clustered indexes. In non-clustered indexes, the index is created on one or more column and then column value is referenced to record in table. So in non-clustered indexes are primarily make two operations i.e. locating key in index and then locating referenced record in table unlike directly searching record in clustered indexes. Due to this fact clustered indexes are a better choice than non-clustered indexes and hence it is recommended to have a clustered index if only one index is required or that the most frequently used column or primary key should be a clustered index.

Clustered indexes also have a few disadvantages. When an update action changes the value of the indexed column, it will essentially require two actions i.e. delete from previous position and then insert at next position as per the updated value.

A composite index is an index on two or more columns, which can be either clustered or non-clustered. Usually composite indexes are not advised, except in the following cases:

- To cover a query: To cover all columns in a query so that the database will not search for the reference in database and just scanning the column will serve the purpose.
- To meet the specific (complex) search criteria e.g. the primary key itself is a combination of two or more columns.

## Some tips for indexing:

| Category | Tips |
|---|---|
| Table column selection | - A column that can be easily indexed should be chosen.
- Numeric values are easier to index than strings and hence indexes on integer values are much faster.
- If an index needs to be created on a string field it should be fixed width and not `varchar`, etc.
- Never create indexes based on dates.
- Dynamics AX has an automatically generated `RecID` for most of the tables. Indexing on that column is advisable if another column does not qualify. |
| Numbered columns (to make composite key) to be included in index | - Try minimizing the use of composite keys. If no other single column qualifies, RecId can be a good choice for indexing.
- The number of columns in an index should be minimized so that a search criterion is as simple as possible.
- A maximum of three columns are recommended for a composite key. |
| Number of indexes | - There is no limit to the maximum number of indexes on a table. However, it is recommended that indexes should be limited to only those columns that are frequently used in the `where` clause of select queries.
- Too many indexes will slow the overall operation as one insert or update operation will result in many insert or update operations due to an insert or update being required in each index files beside table. |
| Index column size | The column or combination of columns on which an index is based is known as the key. The key size should be as small as possible to unleash the benefits of indexing in performance optimization. The key size can be reduced by:
- Using the minimum number of columns in the index
- Using the minimum data size of the column used as index key
- Using numeric keys instead of other data types such as varchar, strings, etc. |
| Distinct index keys | Index keys should be chosen so that the duplicated values for the key are minimal. The best case is that the key is unique. |
