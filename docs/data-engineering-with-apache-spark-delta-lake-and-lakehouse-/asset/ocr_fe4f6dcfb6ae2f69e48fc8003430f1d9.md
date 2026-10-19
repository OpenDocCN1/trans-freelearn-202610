This results in the following output:

```
%sql
SELECT count(*) FROM store_orders;
```

Figure 6.30 – Checking the count of rows in sales_orders

+   2. Now, we will read the incrementally ingested data on the second iteration of the electroniz_batch_ingestion_pipeline run. For example, when we ran the pipeline in Chapter 5, Data Collection Stage – The Bronze Layer, the 2021/04/28/19 folder structure stored the incremental data for the second run. This folder contains data for 200 new rows in the sales_orders table that are ready to be merged in the delta table:

```
ADLS_FOLDER="2021/04/28/19"
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+STORE_SALES_FOLDER+"/\[dbo\].\["+TABLE+"\]/"+ADLS_FOLDER
df_read_data_hour2 = spark.read.csv(BRONZE_TABLE_PATH, schema=schema )
display(df_read_data_hour2)
```

This results in the following output:

```
ADLS_FOLDER="2021/04/28/19"
BRONZE_TABLE_PATH="wasbs://"+BRONZE_LAYER_NAMESPACE+"@"+STORAGE_ACCOUNT+".blob.core.windows.net/"+STORE_SALES_FOLDER+"/\[dbo\].\["+TABLE+"\]/"+ADLS_FOLDER
df_read_data_hour2 = spark.read.csv(BRONZE_TABLE_PATH, schema=schema )
display(df_read_data_hour2)
```

| order_number | customer_id | product_id | order_date | units | sale_price | currency | order_mode | updated_at |
| 2501 | 568 | 3 | 04/05/2020 | 10 | 68.07 | USD | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2502 | 825 | 4 | 03/29/2017 | 7 | 50.44 | INR | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2503 | 616 | 10 | 07/27/2019 | 6 | 82.61 | USD | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2504 | 606 | 10 | 12/05/2018 | 9 | 77.29 | EURO | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2505 | 550 | 2 | 06/30/2020 | 8 | 98.87 | INR | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2506 | 848 | 3 | 10/28/2018 | 2 | 41.32 | USD | EDIT | 2021-04-28T18:25:41.120+0000 |
| 2507 | 810 | 8 | 11/08/2020 | 7 | 89.12 | EURO | EDIT | 2021-04-28T18:25:41.120+0000 |
Showing all 200 rows.

Figure 6.31 – Incremental data for the second iteration of electroniz_batch_ingestion_pipeline