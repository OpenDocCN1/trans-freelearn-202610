The following code joins two tables using the cust_id column:

```
df_left.join(df_right,on="cust_id",how="full").show()
df_left.join(df_right,on="cust_id",how="fullouter").show()
df_left.join(df_right,on="cust_id",how="full_outer").show()
```

The output matches what is shown in Figure 2.17.

# The cross join

A cross join returns the Cartesian product of two relations.

The cross join is an extremely expensive data join operation, often requiring large memory. Hence, it needs to be enabled before using it, as shown in the following code:

```
spark.conf.set("spark.sql.crossJoin.enabled", "true")
df_left.crossJoin(df_right).show()
```

This output is shown in Figure 2.18, which explains the cross join operation. In the following figure, there are two tables, named Left Table-1-1 and Right table-1-1. The cust_id column exists in both the tables and, hence, can be used as a join column.

![](bbox=[0.115, 0.468, 0.85, 0.745])

Figure 2.18 – An example of a cross join

The following code demonstrates the joining of two tables using joins such as semi, leftsemi, and left_semi:

```
df_left.join(df_right,on="customer_id", how="semi").show()
```