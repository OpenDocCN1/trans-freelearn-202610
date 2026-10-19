
```
%sql
DROP TABLE IF EXISTS `inventory`; 
```

OK
Command took 8.08 seconds -- by datauniversity.ca@gmail.com at 2021-07-12, 9:09:38 p.m. on curationcluster

```
DELTA_TABLE_WRITE_PATH="wasbs://"+SILVER_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+INVENTORY_FOLDER
df_read_inventory1.write.format("delta").option("path", DELTA_TABLE_WRITE_PATH).saveAsTable(TABLE)
```

(4) Spark Jobs
Command took 10.73 seconds -- by datauniversity.ca@gmail.com at 2021-07-12, 9:10:15 p.m. on curationcluster

```
%sql
SELECT * FROM `inventory`; 
```

(1) Spark Jobs

|  | inventory_date | product | inventory |
|---|---|---|---|
| 1 | 2021-04-18 | Watch | 100 |
| 2 | 2021-04-18 | Vacuum | 500 |
| 3 | 2021-04-18 | Airconditioner | 45 |
| 4 | 2021-04-18 | Microwave | 34 |
| 5 | 2021-04-18 | DVD | 6789 |
| 6 | 2021-04-18 | TV | 43 |
| 7 | 2021-04-18 | Keyboard | 564 |
Showing all 11 rows.

Figure 10.13 – Creating the inventory Delta table using the 2021-04-18 file

10. Notice that the inventory table schema in Delta Lake now has three columns; that is, inventory_date, product, and inventory:

```
%sql
DESCRIBE `inventory`; 
```

|  | col_name | data_type | comment |
|---|---|---|---|
| 1 | inventory_date | string |  |
| 2 | product | string |  |
| 3 | inventory | int |  |
| 4 |  |  |  |
| 5 | # Partitioning |  |  |
| 6 | Not partitioned |  |  |
Showing all 6 rows.

Figure 10.14 – Checking the inventory Delta table's schema

- Next, run Cmd8. These instructions will create a Spark DataFrame named df_read_inventory2 using the Day 2=2021-04-19 inventory file:

```
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+INVENTORY_FOLDER
df_read_inventory2 = spark.read.csv(BRONZE_TABLE_PATH, header='True', inferSchema='True')
display(df_read_inventory2)
```

(3) Spark Jobs

df_read_inventory2: pyspark.sql.dataframe.DataFrame = [inventory_date: string, product: string ... 2 more fields]

|  | inventory_date | product | inventory | warehouse |
|---|---|---|---|---|
| 1 | 2021-04-19 | Watch | 100 | Arizona |
| 2 | 2021-04-19 | Vacuum | 500 | New York |
| 3 | 2021-04-19 | Airconditioner | 45 | Texas |
| 4 | 2021-04-19 | Microwave | 34 | New York |
| 5 | 2021-04-19 | DVD | 6789 | Texas |
| 6 | 2021-04-19 | TV | 43 | New York |
| 7 | 2021-04-19 | Keyboard | 564 | New York |
Showing all 11 rows.

Figure 10.15 – Reading the 2021-04-19 inventory file

Notice that df_read_inventory2 has four columns: inventory_date, product, inventory, warehouse.

- Run Cmd9. This instruction will try to merge df_read_inventory2 into the existing inventory Delta table:

```
df_read_inventory2.write.format("delta").mode("append").save(DELTA_TABLE_WRITE_PATH)
```

AnalysisException: A schema mismatch detected when writing to the Delta table (Table ID:

AnalysisException
Traceback (most recent call last)
<command-2814988441641260> in <module>
----> 1 df_read_inventory2.write.format("delta").mode("append").save(DELTA_TABLE_WRITE_PATH)

Figure 10.16 – Schema mismatch exception

Here, the instruction ran into an exception. If you scroll down the error window, you will see the following output:

```
AnalysisException: A schema mismatch detected when writing to the Delta table (Table ID: 2990042c-3e69-4a50-b75c-70f8d2d9ef25).
To enable schema migration using DataFrameWriter or DataStreamWriter, please set:
'.option("mergeSchema", "true")'.
For other operations, set the session configuration
spark.databricks.delta.schema.autoMerge.enabled to "true". See the documentation
specific to the operation for details.

Table schema:
root
-- inventory_date: string (nullable = true)
-- product: string (nullable = true)
-- inventory: integer (nullable = true)

Data schema:
root
-- inventory_date: string (nullable = true)
-- product: string (nullable = true)
-- inventory: integer (nullable = true)
-- warehouse: string (nullable = true)
```

Figure 10.17 – Schema mismatch between the 2021-04-18 and 2021-04-19 inventory files

This is the expected behavior. The Delta table has correctly detected a schema mismatch between the incoming data and the pre-existing Delta table. By default, Delta Lake will throw an exception if such a mismatch is detected. The real question is how to deal with such a situation.

11. Now, run Cmd10 and Cmd11. Notice that a new option has been added in the merge command. mode=append overrides the default behavior and instructs Delta Lake to proceed with the data merge by adjusting the schema:

```
df_read_inventory2.write.format("delta").mode("append").option("mergeSchema", "true").save(DELTA_TABLE_WRITE_PATH)
```

```
%sql
DESCRIBE inventory;
```

|  | col_name | data_type | comment |
|---|---|---|---|
| 1 | inventory_date | string |  |
| 2 | product | string |  |
| 3 | inventory | int |  |
| 4 | warehouse | string |  |
| 5 |  |  |  |
| 6 | # Partitioning |  |  |
| 7 | Not partitioned |  |  |
Showing all 7 rows.

Figure 10.18 – Creating the inventory Delta table using the 2021-04-19 file using mode=append

Notice that this time, an exception was not raised. Instead, Delta Lake calmly adjusted the schema of the pre-existing table by adding a new column to it.

> **Important**
> The decision to either raise an exception or use mode=append in the Delta table's merge commands should be discussed and agreed upon with the customer.

12. Run Cmd12. Note how Delta Lake has adjusted the data after merging the data and schema. All data before Day 2=2021-04-19 has been marked as NULL because the new column warehouse did not exist before:

```
Cmd 12
1   %sql
2   SELECT * FROM inventory;
```

(2) Spark Jobs

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_352_0.png)

Figure 10.19 – Checking the data in the inventory Delta table

Now, let's see what happens when we try to merge the Day 3=2021-04-20 inventory file with the inventory table. You may recall that on this day, a new column (city) was added but a previous column was deleted (warehouse).

Run Cmd13. These instructions will create a Spark DataFrame named df_read_inventory3 using the Day 3=2021-04-20 inventory file:

```
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+INVENTORY_FOLDER
df_read_inventory3 = spark.read.csv(BRONZE_TABLE_PATH, header='True', inferSchema='True')
display(df_read_inventory3)
```

(3) Spark Jobs

df_read_inventory3: pyspark.sql.dataframe.DataFrame = [inventory_date: string, product: string ...]

| inventory_date | product | inventory | city |
|---|---|---|---|
| 1 | 2021-04-20 | Watch | 100 | Phoenix |
| 2 | 2021-04-20 | Vacuum | 500 | New York |
| 3 | 2021-04-20 | Airconditioner | 45 | Dallas |
| 4 | 2021-04-20 | Microwave | 34 | New York |
| 5 | 2021-04-20 | DVD | 6789 | Dallas |
| 6 | 2021-04-20 | TV | 43 | New York |
| 7 | 2021-04-20 | Keyboard | 564 | New York |
Showing all 11 rows.

Figure 10.20 – Reading the 2021-04-20 inventory file

13. Run Cmd14. This instruction will try to merge df_read_inventory3 in the existing inventory Delta table.

Run Cmd15. Notice how Delta Lake was able to adjust the schema once again. Even though the warehouse column was deleted in the incoming file, it remains in the Delta table:

```
df_read_inventory3.write.format("delta").mode("append").option("mergeSchema", "true").save(DELTA_TABLE_WRITE_PATH)
```

(4) Spark Jobs

Command took 3.66 seconds -- by datauniversity.ca@gmail.com at 2021-07-12, 9:43:51 p.m. on curationcluster

```
%sql
DESCRIBE inventory;
```

| col_name | data_type | comment |
|---|---|---|
| 1 | inventory_date | string |  |
| 2 | product | string |  |
| 3 | inventory | int |  |
| 4 | warehouse | string |  |
| 5 | city | string |  |
| 6 |  |  |  |
| 7 | # Partitioning |  |  |
Showing all 8 rows.

Figure 10.21 – Creating the inventory Delta table using the 2021-04-20 file using mode=append

14. Run Cmd16:

```
%sql
SELECT * FROM inventory;
```

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_354_0.png)

Figure 10.22 – Checking the data in the inventory Delta table

This time, after merging data from Day 3=2021-04-20, the warehouse column gets defaulted to NULL, and the new city column has been added to the inventory table.

With that, you've seen how Delta Lake can transparently adjust the data and schema each time a schema change is detected. Hopefully, this example was able to effectively highlight how schema evolution can be managed in modern data engineering pipelines. We will now look at sharing data. This is quickly becoming a necessity for organizations who are wanting to harness the power of data in monetary terms.

## Sharing data

In Chapter 1, The Story of Data Engineering and Analytics, we discussed the power of data. This has enabled organizations to realize revenue diversification using data monetization. But this dream cannot be effectively realized without sharing data with external parties. In the past, organizations used several data-sharing mechanisms such as emails, SFTP, APIs, cloud storage, and hard drives:
