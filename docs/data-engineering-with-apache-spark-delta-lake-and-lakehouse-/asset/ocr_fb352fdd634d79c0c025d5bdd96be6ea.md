This results in the following output:

```
%sql
DESCRIBE HISTORY store_orders;
```

| version | timestamp | userid | userName | operation |
|---|---|---|---|---|
| 3 | 2021-05-04T15:09:30.000+0000 | 2160993539918818 |  | RESTORE |
| 2 | 2021-05-04T15:09:14.000+0000 | 2160993539918818 |  | DELETE |
| 1 | 2021-05-04T15:09:09.000+0000 | 2160993539918818 |  | UPDATE |
| 0 | 2021-05-04T15:09:03.000+0000 | 2160993539918818 |  | CREATE TABLE AS SELECT |
Showing all 4 rows.

Figure 6.29 – Transaction history of the sales_orders table after the restore operation

Notice that version 3 of the table shows the operation=RESTORE, indicating that the table was restored to a previous state.

During the scheduled ingestion process, data is inserted and updated repeatedly. Delta Lake provides a convenient way of merging data in existing tables using upserts.

# Performing upserts of data

Delta Lake also supports the upsert operation using the MERGE clause. The idea of the upsert is simply to merge data in the existing table with new data originating from the source. It is very common in a lakehouse to ingest incremental data and merge it with the pre-existing table in Delta Lake. The MERGE process is repeated each time new data gets ingested and is instrumental in creating the single truth of data in the silver layer of the lakehouse.

> Important Note
> An UPSERT operation is a mix of updates and inserts in the same operation. Using the MERGE clause, you can perform an insert in a table if it does not exist or simply update existing data if it does.

+   1. Before we start the MERGE process, let's find out the current number of rows in the store_orders table:

```
%sql
SELECT count(*) FROM store_orders;
```