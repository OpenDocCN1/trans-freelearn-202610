```
["customer_id", "name"])
df_right.show()
```

The following code joins two tables using the cust_id column:

```
df_left.join(df_right, on="cust_id", how="inner") .show ()
```

We get the following output:

```
+-------+----------+------+-----+
|cust_id|order_id |amount|name |
+-------+----------+------+-----+
|      1|     1001|   100| john|
|      1|     1004|   200| john|
|      2|     1002|   200| mike|
|      3|     1003|   300| tony|
+-------+----------+------+-----+
```

# The left join

A left join returns all values from the left table and the matched values from the right table, or it appends NULL if there is no match. It is also referred to as a left outer join. LEFT JOIN and LEFT OUTER JOIN are equivalent.

Figure 2.15 explains the left join operation. In the following figure, there are two tables, named Left_Table-1 and Right_table-1. The cust_id column exists in both the tables and, hence, can be used as a join column.

# Left Join

![](bbox=[0.13, 0.595, 0.3, 0.665])

| order_id | cust_id | amount |
|---|---|---|
| 1001 | 1 | 100 |
| 1002 | 2 | 200 |
| 1003 | 3 | 300 |
| 1004 | 1 | 200 |
| 1005 | 6 | 200 |

| cust_id | name |
|---|---|
| 1 | john |
| 2 | mike |
| 3 | tony |
| 4 | kent |

| cust_id | order_id | amount | name |
|---|---|---|---|
| 6 | 1005 | 200 | None |
| 1 | 1001 | 100 | john |
| 1 | 1004 | 200 | john |
| 3 | 1003 | 300 | tony |
| 2 | 1003 | 200 | mike |

Join column Unmatched record Records that exist in the left table and matching records from the right table

Figure 2.15 – An example of a left join