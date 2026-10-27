You can either schedule the ingestion of each source system, each with a `load_all_XXX` macro, one after the other, or you can do them in parallel and then schedule the transformations.

In any case, you will read from the landing tables by defining them as the sources for your staging models.

Another practical consideration, especially if you have everyday full exports from your sources, is to remove the old data that has been loaded from the landing tables.

To achieve this, you can simply add a DELETE query after the COPY INTO command. The DELETE query would look like this:

```
DELETE FROM landing_table
WHERE <where_clause>
```

There are a few approaches that you can follow for the WHERE clause:

+   - Keep the last *N* ingestion batches. Every time we load data into the landing table, all the rows have the same `INGESTION_TS_UTC`, so you can write a clause like this:

```
WHERE  INGESTION_TS_UTC  NOT  IN
    (SELECT DISTINCT INGESTION_TS_UTC
       FROM <landing>
       ORDER BY INGESTION_TS_UTC DESC
       LIMIT N)
```

+   - Delete the data that was ingested more than *N* days ago. Here, you can write a clause like this:

```
WHERE
    INGESTION_TS_UTC  <  DATEADD(DAY, -N, CURRENT_DATE)
```

+   - You could also write a WHERE clause that is referencing the HIST table where the data from the landing table will be stored and delete what was loaded before the max `INGESTION_TS_UTC` from the HIST table.
+   
+     While tempting and allowed by SQL, this is not a great idea you will create a cycle where the landing table depends on the HIST table and the HIST table on the landing table. This can complicate the process of running and maintaining the project.

Note that while deletions are quite slow in general in columnar databases, if you delete based on `INGESTION_TS_UTC`, you will pretty much delete according to the micro-partitions created during each daily load, so you will get very good performance.

# History patterns

In the previous chapters, we have seen how to define proper HKEY and HDIFF fields, use the `save_history` macro to store the versions of the entities that we see over time, and use the `current_from_history` macro to read the current version for each instance of the entity out of the history table.