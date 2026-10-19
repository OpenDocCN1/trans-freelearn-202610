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

![](bbox=[0.19, 0.44, 0.82, 0.64])

Figure 10.19 – Checking the data in the inventory Delta table

Now, let's see what happens when we try to merge the Day 3=2021-04-20 inventory file with the inventory table. You may recall that on this day, a new column (city) was added but a previous column was deleted (warehouse).

Run Cmd13. These instructions will create a Spark DataFrame named df_read_inventory3 using the Day 2=2021-04-120 inventory file: