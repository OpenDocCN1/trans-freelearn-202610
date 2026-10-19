10. We are now ready to read the store_orders table from the Azure data lake storage. You may recall that we had previously ingested this table using electroniz_batch_ingestion_pipeline. Copy the following commands into the third cell of the notebook:

```
ORDERS_SCHEMA = [
    ('order_number', IntegerType()),
    ('customer_id', IntegerType()),
    ('product_id', IntegerType()),
    ('order_date', StringType()),
    ('units', IntegerType()),
    ('sale_price', FloatType()),
    ('currency', StringType()),
    ('order_mode', StringType()),
    ('updated_at', TimestampType())
]
fields = [StructField(*field) for field in ORDERS_SCHEMA]
schema = StructType(fields)
df_read_data_hour1 = spark.read.csv(BRONZE_TABLE_PATH, schema=schema)
display(df_read_data_hour1)
```

Notice how the schema for the store_orders table is being defined before reading its data in a Spark DataFrame.