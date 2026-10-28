# Creating new measures in the summary table

Now, it is time to create a new measure within the **Internet Sales Aggregated** table. To keep this scenario simple, we’ll create one new measure that calculates the summation of the **Sales** column from the **Internet Sales Aggregated** table. The DAX expression for this is as follows:

```
Sales Agg = SUM('Internet Sales Aggregated'[Sales])
```

The **Sales Agg** measure calculates the sales amount. Remember, the **Internet Sales Aggregated** table is derived from the **Internet Sales** table. Therefore, the **Sales Agg** measure uses the same calculation as the **Internet Sales** measure from the **Internet Sales** table. Here is the DAX expression of the **Internet Sales** measure:

```
Internet Sales = SUM('Internet Sales'[SalesAmount])
```

The main differences between the **Sales Agg** measure and the **Internet Sales** measure are as follows:

+   - **Sales Agg** calculates the sales amount over a small table (**Internet Sales Aggregated**).
+   - **Internet Sales** calculates the same in a larger table (**Internet Sales**).
+   - As a result of the preceding points, the **Sales Agg** measure performs much better than **Internet Sales** when many concurrent users use the report.
+   - The users can filter the **Internet Sales** values by **Customer**, **Product**, and **Date**, but they can only filter the **Sales Agg** values by **Date**.

Now that we’ve created the **Sales Agg** measure, we are ready for the next step in our design: creating a new control measure to detect the level of grain selected by the user.