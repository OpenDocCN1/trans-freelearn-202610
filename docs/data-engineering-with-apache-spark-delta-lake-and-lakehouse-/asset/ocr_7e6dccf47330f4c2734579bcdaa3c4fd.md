```
if tables=="store_customers":
    df_table_incremental = spark.read.csv(bronze_table_path, schema=schema_stores )
    #display(df_table_incremental)
    df_table_curated = df_table_incremental.withColumn('email_curated',curate_email_udf('email')).drop('email').withColumnRenamed('email_curated', 'email')
    df_table_curated = df_table_curated.withColumn('country_curated',curate_country_udf('country')).drop('country').withColumnRenamed('country_curated', 'country')
    df_table_curated = df_table_curated.withColumn('phone_masked',mask_udf('phone')).drop('phone').withColumnRenamed('phone_masked', 'phone')
```

Figure 7.46 – Making country data uniform in store_customers

Note the country data in store_customers now exists in an abbreviated form:

# Verify country in store_customers

```
1   %sql
2   select customer_name,country from store_customers
```

(2) Spark Jobs

|  | customer_name | country |
|---|---|---|
| 1 | Aubrey Norris | UK |
| 2 | Bruno Hebert | UK |
| 3 | Serina Serrano | UK |
| 4 | Ivory Lester | UK |
| 5 | Tad Mcintyre | UK |
| 6 | Carson Gonzales | UK |
| 7 | Carlv Yates | UK |

Figure 7.47 – Verifying the uniform country data in store_customers

Notice how the same PySpark function is used to curate the country column in e-commerce transactions. In the cell titled Curate e-commerce Sales Data - standardize, mask and merge, notice the following code that curates country:

> Important note
> The idea of common functions not only promotes code re-usability but comes in extremely handy if data needs to be replayed from scratch using a modified logic within the function.

```
df_data_values = df_data_values.withColumn('phone_masked',mask_udf('phone')).drop('phone').withColumnRenamed('phone_masked', 'phone')
df_data_values = df_data_values.withColumn('address_masked',mask_udf('address')).drop('address').withColumnRenamed('address_masked', 'address')
df_data_values = df_data_values.withColumn('country_curated',curate_country_udf('country')).drop('country').withColumnRenamed('country_curated', 'country')
```

Figure 7.48 – Verifying the uniform country data in e-commerce transactions