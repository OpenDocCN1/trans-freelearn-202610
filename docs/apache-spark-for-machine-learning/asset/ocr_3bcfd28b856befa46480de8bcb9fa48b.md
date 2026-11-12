Each type of join serves a different purpose and is chosen based on the data requirements of the specific task at hand. Understanding how these joins work is fundamental to relational database operations and data analysis tasks.

Let's now discuss the joins shown in Figure 2.13 in detail.

# The inner join

The inner join is the default join in Spark SQL. It selects rows that have matching values in both tables.

Figure 2.14 explains the inner join operation. As seen in the figure, there are two tables, named Left_Table and Right_table. The cust_id column exists in both the tables and, hence, can be used as a join column.

![](bbox=[0.13, 0.32, 0.82, 0.62])

The following code creates a Spark DataFrame, df_left, containing three columns, named order_id, customer_id, and amount:

```
df_left = spark.createDataFrame(
    [(1001,1,100),(1002,2,200),
    (1003,3,300),(1004,1,200),(1005,6,200)] ,
    ["order_id","customer_id","amount"])
df_left.show()
```

The following code creates a Spark DataFrame, df_right, containing two columns named customer_id and name:

```
df_right = spark.createDataFrame(
    [(1,"john"), (2,"mike"), (3,"tony"), (4,"kent")] ,
```