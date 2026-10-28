
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

- **Sales Agg** calculates the sales amount over a small table (**Internet Sales Aggregated**).
- **Internet Sales** calculates the same in a larger table (**Internet Sales**).
- As a result of the preceding points, the **Sales Agg** measure performs much better than **Internet Sales** when many concurrent users use the report.
- The users can filter the **Internet Sales** values by **Customer**, **Product**, and **Date**, but they can only filter the **Sales Agg** values by **Date**.

Now that we’ve created the **Sales Agg** measure, we are ready for the next step in our design: creating a new control measure to detect the level of grain selected by the user.

after hiding the **Internet Sales Aggregated** table and the **Internet Sales** measure from the **Internet Sales** table:

![](img/5d2bce74fd6fceebed14fd1488bf7088_534_0.png)

Figure 10.4: The data model after hiding the Internet Sales Aggregated table and the Internet Sales measure

We have everything sorted, so it is time to test the solution in the data visualization layer. We need to ensure that the **Internet Sales** measure and the **Internet Sales Total** measure always result in the same values. We also need to ensure that when the user selects any columns from either the **Product** table or the **Customer** table, the **Internet Sales Total** measure uses the base table (the **Internet Sales** table). In our example, we create a report page that shows the **Internet Sales** and **Internet Sales Total** measures side by side. The following image shows the report page:

![](img/5d2bce74fd6fceebed14fd1488bf7088_535_0.png)

Figure 10.5: Testing the aggregation in data visualization

As the preceding image shows, we have a card visual at the top of the page showing the table used in the calculation within the table visual (in the middle of the report page). We used the following measure in the card visual:

```
Test Grain Selection Control =
    IF(
        OR(ISFILTERED('Product'), ISFILTERED('Customer'))
        , "Internet Sales"
        , "Internet Sales Aggregated"
    )
```

The preceding expression checks if the Product or Customer tables are filtered. If the result is TRUE, then the Internet Sales table is used; otherwise, the Internet Sales Aggregated table is used.

The following image shows the same report page when the user selects a Product Category from the slicer on the left-hand side of the report:

![](img/5d2bce74fd6fceebed14fd1488bf7088_536_0.png)

Figure 10.6: The test visuals after selecting a Product Category from the slicer

As the preceding image shows, the aggregation is working correctly.

So far, we’ve implemented a simple use case for implementing aggregations in Power BI Desktop. But there are many other use cases for aggregations. In the next section, we’ll elevate the scenario’s complexity.

## Implementing aggregation at the Year and Month level

This time, we want to create another summary table that summarizes the Internet Sales table at the Year and Month granular levels based on the OrderDateKey column. We also want to include row counts of Internet Sales as an aggregated column. The steps for implementing this scenario are the same as in the previous scenario. The only difference is in the data preparation layer summarizing the Internet Sales data and changing the granularity of the data from Date to Year and Month. Let’s have a closer look at the data preparation layer. To summarize the Internet Sales data at

# Figure 10.8: Summarizing the Internet Sales table at the Year and Month level

We can now apply the changes within the Power Query Editor window, which adds **Internet Sales Aggregated Year Month** as a new table. Moving forward with our implementation, we must create a relationship between the **Internet Sales Aggregated Year Month** table and the **Date** table. The following image shows the relationship once created:

![](img/5d2bce74fd6fceebed14fd1488bf7088_540_0.png)

Figure 10.9: Creating a relationship between Internet Sales Aggregated Year Month and Date

We set the relationship’s **Cardinality** to **Many to one** and **Cross-filter direction** to **Single**.

When we create the preceding relationship, Power BI automatically detects the relationship as one-to-one. Conceptually, this is correct. Each row of the **Internet Sales Aggregated Year Month** table occurs on the first day of each month related to only one row in the **Date** table. However, we need to change the relationship’s cardinality from one-to-one to many-to-one to avoid filter propagation from the **Internet Sales Aggregated Year Month** table to the **Date** table.

The next step is to create a new measure using the following DAX expression in the **Internet Sales Aggregated Year Month** table:

```
Sales Year Month = SUM('Internet Sales Aggregated Year Month'[Sales])
```

Now, we need to create a new control measure to detect the level of detail the user selects, not only at the **Product** and **Customer** levels but also at the `Date` level in the **Internet Sales** table. The following DAX expression caters to this:

```
Internet Sales Agg =
    IF(
ISFILTERED('Product') ||
ISFILTERED('Customer') ||
ISFILTERED('Date'[Full Date])
    , [Internet Sales]
    , [Sales Year Month]
)
```

Let us create a test measure that evaluates the `IF()` function in the preceding expression. The following DAX expression evaluates if either the **Product** and **Customer** tables or the **Full Date** column from the **Date** table are filtered, which results in "Internet Sales" (a text value); otherwise, the output is "Internet Sales Aggregated" (a text value). With the following test measure, we can get a better understanding of how the **Internet Sales Agg** measure works:

```
Internet Sales Agg Test =
    IF(
ISFILTERED('Product') ||
ISFILTERED('Customer') ||
ISFILTERED('Date'[Full Date])
    , "Internet Sales"
    , "Internet Sales Aggregated"
)
```

The last bit is to test the aggregation on a report page. The following image shows the report page created to test the aggregation results before the user puts any filters on the **Product** or **Customer** tables:

![](img/5d2bce74fd6fceebed14fd1488bf7088_542_0.png)

Figure 10.10: Testing the aggregation before filtering by the Product or Customer tables

Note the Total value for the Internet Sales Agg Test measure shows Internet Sales Aggregated. As the preceding image shows, if we do not filter by Product or Customer, the Internet Sales Agg measure calculates the results for the Year and Month levels. But at the Date level, the Internet Sales measure calculates the values.

This technique shows its value, especially when many concurrent users use the same report. However, we must be mindful of the following side effects:

- The summary tables will increase memory and storage consumption.
- The summary tables will also increase the data refresh time.
- The development time also increases as we need to create the summary table, create the relationships, and create the measures.

Now that we have implemented aggregations on top of non-DirectQuery data sources, it is time to look at the Agg Awareness feature.
