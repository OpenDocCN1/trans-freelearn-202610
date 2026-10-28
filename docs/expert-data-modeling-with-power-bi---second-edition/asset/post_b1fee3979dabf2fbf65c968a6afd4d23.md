
Now that we have created the textual measures, we can use them to format the visuals conditionally (if supported). The following visuals currently support conditional formatting:

| Stacked Column Chart | Clustered Bar Chart | Clustered Column Chart | 100% Stacked Bar Chart |
|---|---|---|---|
| 100% Stacked Column Chart | Line and Stacked Column Chart | Line and Clustered Column Chart | Ribbon Chart |
| Funnel Chart | Scatter Chart | Treemap Chart | Gauge |
| Card | KPI | Table | Matrix |

Figure 9.23: Power BI default visuals supporting conditional formatting

The following steps show how to format a clustered column chart conditionally:

- 1. Put a **Clustered column chart** on a new report page.
- 2. Choose the **Year-Month** column from the **Date** table from the visual’s **Axis** dropdown.
- 3. Choose the **Sales MoM%** measure from the visual’s **Values** dropdown.
- 4. Choose the **Sales MoM% Description** measure from the **Tooltips** dropdown.
- 5. Switch to the **Format** tab from the **Visualizations** pane.
- 6. Expand the **Columns**.
- 7. Expand **Colors**.
- 8. Click the **fx** button.
- 9. Select **Field value** from the **Format by** drop-down menu.
- 10. Select the **Sales MoM% Colour** measure from the **Based on field** menu.
- 11. Click **OK**.

The following image shows the preceding steps:

![](img/5d2bce74fd6fceebed14fd1488bf7088_498_0.png)

Figure 9.24: Applying dynamic conditional formatting on a clustered column chart

The following image shows the clustered column chart after the preceding settings:

![](img/5d2bce74fd6fceebed14fd1488bf7088_498_1.png)

# Figure 9.25: The clustered column chart after it has been conditionally formatted

With this technique, we can create compelling data visualizations that can quickly provide many insights about the data. For instance, the following image shows a report page analyzing sales by date. Add a matrix showing Internet Sales by Month and Day:

![](img/5d2bce74fd6fceebed14fd1488bf7088_499_0.png)

# Figure 9.26: Color-coded Sales report

Use the Sales MoM% Colour measure to color-code the matrix. Every cell of the matrix shows the sales amount by weekday for the entire month in this report. However, the color of the cell shows the comparison of current sales against the same weekday last month. For instance, if we look at the 2012-Jun sales, we quickly see that the Monday sales were excellent compared to 2011-Jun.

The preceding report page might not be a perfect example of a high-standard data visualization. However, without a doubt, it provides many more insights than a similar report page without color coding. In real-world scenarios, we might have more colors for negative numbers to show the severity of the metric we are analyzing. We will leave this to you as an exercise on how to use this technique to create very professional-looking reports.

# Avoiding calculated columns when possible

Creating calculated columns is one of the most essential and powerful features of DAX. Calculated columns, as the name suggests, are computed based on a formula; therefore, the calculated column values are unavailable in the source systems or the Power Query layer. The values of the calculated columns are computed during the data refresh and then stored in memory. It is important to note that the calculated columns reside in memory unless we unload the whole data model from memory, which in Power BI means when we close the file in Power BI Desktop or switch to other content in the Power BI Service. Calculated columns, after creation, are just like any other columns, so we can use them in other calculated columns, measures, calculated tables, or for filtering the visualization layer.

A common approach of developers is to use calculated columns to divide complex equations into smaller chunks. That is why we suggest avoiding the excessive use of calculated columns. The general rules of thumb for using calculated columns are as follows:

- - Create a calculated column if you use it in filters.
- - Even though you need to use the calculated column in filters, consider creating the new column in the Power Query layer or the source when possible.
- - Do not create calculated columns if you can create a measure with the same results.
- - Always think about the data cardinality when creating calculated columns. The higher the cardinality, the lower the compression and the higher the memory consumption.
- - Always have a strong justification for creating a calculated column, especially when dealing with large models.

![](img/5d2bce74fd6fceebed14fd1488bf7088_502_0.png)

Figure 9.27: View Metrics in DAX Studio

As you see in the preceding image, the **Gross Profit** row size is 15,448 bytes (approximately 15 KB), with the cardinality of 45 consuming 0.31% of the table size. The **Internet Sales** table is a small table with 60,398 rows. So, we can imagine how the column size can grow in larger tables.

While the process of creating a calculated column and then getting the summation of the calculated column is legitimate, it is not the preferred method. We can compute `Total Gross Profit` in a measure with the following DAX expression:

```
Total Gross Profit Measure = SUMX('Internet Sales',
'Internet Sales'[SalesAmount] - 'Internet Sales'[TotalProductCost])
```

The difference between the two approaches is that the values of the `Gross Profit` calculated row are computed at the table refresh time. It resides in memory, while its measure counterpart aggregates the gross profit when we use it in a visual. So when we use the **Product Category** column from the **Product** table in a **clustered column chart** and the `Total Gross Profit Measure`, the values of the `Total Gross Profit Measure` are aggregated for the number of product categories in memory. The following image

shows **Total Gross Profit Measure by Product Category** in a **clustered column chart:**

![](img/5d2bce74fd6fceebed14fd1488bf7088_503_0.png)

Figure 9.28: Total Gross Profit Measure by Product Category in a clustered column chart

As the preceding image shows, the `Total Gross Profit Measure` is aggregated in the `Product Category` level with only three values, so the calculation is superfast with minimal memory consumption.

# Organizing the model

There are usually several roles involved in a Power BI project in real-world enterprise BI scenarios. From a Power BI development perspective, we might have data modelers, report writers, quality assurance specialists, support specialists, and so on. The data modelers are the ones who make the data model available for all other content creators, such as report writers. So, making a model that is as organized as possible is essential. This section looks at several ways to organize our data models.

# Hiding insignificant model objects

One way to keep our model tidier is to hide all insignificant objects from the data model. We often have some objects in the data model that are not used elsewhere. However, we cannot remove them from the data model as we may require them in the future. So, the best practice is to hide all those objects unless they are going to serve a business requirement. The following sections discuss the best candidate objects for hiding in our data model.

# Hiding unused fields and tables

There are many cases when we have some fields (columns or measures) or tables in the data model that are not used elsewhere. Unused fields are the measures or columns that fulfill the following criteria:

- - Are not used in any visuals on any report pages
- - Are not used within the **Filters** pane
- - No measures, calculated columns, calculated tables, or calculation groups referencing those fields
- - No roles within the row-level security reference those fields

If we have some fields falling in all of the preceding categories, it is highly recommended to hide them in the data model. The idea is to keep the data model as tidy as possible so that you can hide some fields that fall into some of the preceding categories, based on your use cases.

Unused tables, on the other hand, are tables with all their fields unused.

While this best practice suggests hiding unused fields and tables, applying it can be time-consuming. If done manually, finding the fields that are not referenced anywhere within the model or are not used in any visuals can be a laborious job. Luckily, some third-party tools can make our lives easier. For instance, we can use **Power BI Documenter**, which can not only find
