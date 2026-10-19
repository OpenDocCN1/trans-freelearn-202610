4. After completion, let's find out the change in the number of rows in the store_orders table after the merge:

```
%sql
SELECT count(*) FROM store_orders;
```

This results in the following output:

Figure 6.33 – Checking the count of rows in sales_orders after the merge of the second iteration

The number of rows has incremented by 200. I am sure you have guessed by now that the incremental rows were merged using the whenNotMatchedInsert clause.

5. Due to the recent merge, the history of the store_orders table will have advanced as well:

```
%sql
DESCRIBE HISTORY store_orders;
```

This results in the following output:

Figure 6.34 – Transaction history of the sales_orders table after the merge operation of the second iteration