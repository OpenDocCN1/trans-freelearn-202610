
`[Product Subcategory] = "Bike Racks")` to `List.Contains(#"Approved Subcategories"[Approved Product Subcategories], [Product Subcategory])`, as shown here:

![](img/5d2bce74fd6fceebed14fd1488bf7088_277_0.png)

Figure 5.33: Changing the condition of the Table.SelectRows function

The Product table is now being filtered by the values of the Approved Product Subcategories column from the Approved Subcategories table.

Let us save the file as a new file: `Chapter 5, Common Data Preparation Steps, Dynamic Filtering, Applied Filter.pbix`.

## Working with Group By

One of the most valuable and advanced techniques in data modeling is creating summary tables. In many scenarios, using this method is very beneficial. We can use this method to manage our Power BI file’s size and improve performance and memory consumption. Summarization is a known technique in data warehousing where we change the granularity of a fact table to a higher grain. But in Power Query, there are other cases where we can use the Group By functionality to cleanse the data. From a data modeling point of view, we summarize a table by grouping it into descriptive columns and aggregating the numeric values.

Let us go through a scenario and see how the Group By functionality works.

In this section, we use the `Chapter 5, Common Data Preparation Steps, Fixed OrderDateTime Data Type.pbix` sample file that we created before. You can download the file from here:

https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/9d6388ffe6e83586a02de81ff959e04c31dbf1da/Samples/Chapter%205/Chapter%205,%20Common%20Data%20Preparation%20Steps,%20Fixed%20OrderDateTime%20Data%20Type.pbix

We want to summarize the **Internet Sales** table into a new **Internet Sales Summary** table with the following conditions:

- Group by **ProductKey**
- Group by **OrderDateTime**
- Sum of **SalesAmount**
- Count of **Internet Sales**
- Average of **OrderQuantity**
- Sum of **TaxAmt**
- Average of **TotalProductCost**

After opening the sample file, open Power Query Editor and follow these steps:

1. Right-click on the **Internet Sales** table.
2. Click **Reference**:

Figure 5.34: Referencing the Internet Sales table

3. Rename the new query to **Internet Sales Summary**.
4. Select the **ProductKey** and **OrderDateTime** columns participating in the Group By action.
5. Click the **Group By** button from the **Transform** tab of the ribbon.
6. Type in `Sales` for **New column name**, select the **Sum** operation, and select the **SalesAmount** column.
7. Click the **Add aggregation** button.
8. Repeat *steps 4 to 7* to add the other aggregations.
9. Click **OK**.

Usually, the summary table has much fewer rows than the original table. In some cases, due to business requirements, we must unload the original table from the data model and only use the summary table.

In our sample, the granularity of the **Internet Sales Summary** table is **Product** and **Date**, containing **23,797** rows, while the **Internet Sales** table has **60,398** rows. The following image shows the number of rows in both tables from the **Data** view:

![](img/5d2bce74fd6fceebed14fd1488bf7088_281_0.png)

Figure 5.37: The Internet Sales Summary table content

So if we create the relationships between the **Product** table and the **Date** table, we can analyze the **Sales** data by **Product** and **Date** over a much smaller table, which translates to better performance.

## Appending queries

There are some scenarios where we get the data with the same structure from different sources, and we want to consolidate it into a single table. In those cases, we need to append the queries. We have two options to append the queries:

- Append the queries to the first query
- Append the queries as a new query

The latter is prevalent when we follow ETL best practices. We unload all the queries, append them as a new query, and load the new query into the data model. Therefore, the unloaded queries work as ETL pipelines. However, it does not mean that the first option is not applicable.

Suppose we have a simple business requirement that can be achieved by appending two or more queries to the first query. In that case, use the first option instead. The critical point to note when we append queries is that the `Table.Combine(tables as list, optional columns as any)` function accepts a list of tables. If the column names in the tables are the same, it appends the data with the same column name, regardless of the columns’ data types. Therefore, if we have two tables and both tables have a **Column1** column, then the data of those two columns, regardless of their data types, gets appended with the same column name.

If the data types do not match, the appended column data type is **any**. Remember, Power Query is case-sensitive. Therefore, if we have the **column1** column in **Table1** and the **Column1** column in **Table2**, the column names are different when we append the two tables. So, after appending, we have a table with both **column1** and **Column1** columns. Let us look at an example.

For this example, we use the `Chapter 5, Common Data Preparation Steps, Append Queries.pbix` sample file, which contains sales data for different years in different file formats. You can download the sample file and its source data from here:

https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/9d6388ffe6e83586a02de81ff959e04c31dbf1da/Samples/Chapter%205/Chapter%205,%20Common%20Data%20Preparation%20Steps,%20Append%20Queries.pbix

We can always find the data sources used in Power BI Desktop by following these steps:

1. Click the **Transform data** dropdown.
2. Click **Data source settings**.

As shown in the following image, we have three separate queries for each year’s sales data, which can be changed by selecting each file and clicking the **Change Source...** button:

![](img/5d2bce74fd6fceebed14fd1488bf7088_283_0.png)

Figure 5.38: Sales data spread across three separate queries coming from different data sources

Change the source files to the downloaded source files on your local drive. After closing the **Data source settings** dialog box, a warning appears, as illustrated in the following image. Click **Apply changes**:

![](img/5d2bce74fd6fceebed14fd1488bf7088_284_0.png)

Figure 5.39: Applying changes after changing the source files in Power BI Desktop

In this scenario, we want to consolidate the queries into a single Sales query. To do so, follow these steps in Power Query Editor:

1. Select the 2012 Sales Data query from the Queries pane.
2. Click the **Append Queries** drop-down button from the **Home** tab of the ribbon.
3. Click **Append Queries as New**.
4. Click **Three or more tables**.
5. Select **2013 Sales Data** and **2014 Sales Data** from the **Available tables** list.
6. Click the **Add >>** button.
7. Click **OK**:

![](img/5d2bce74fd6fceebed14fd1488bf7088_284_1.png)

Figure 5.40: Appending queries as a new query

The preceding steps create a new query named **Append1**.

8. Rename the query to **Sales**.
9. Unload all the queries that we appended by right-clicking on **2012 Sales Data**.
10. Untick the **Enable load** option as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_285_0.png)

Figure 5.41: Renaming the Append1 query and unloading the original queries

Repeat the preceding process to disable loading for the **2013 Sales Data** and **2014 Sales Data**.

The results of the preceding steps raise an issue with the data. The following screenshot highlights this issue:

## Merge

Select a table and matching columns to create a merged table.

**Product**

| ProductKey | ProductAlternateKey | ProductSubcategoryKey | WeightUnitMeasureCode | SizeUnitMeasureCode |
|---|---|---|---|---|
| 1 | AR-5381 | null | null | null |
| 2 | BA-8327 | null | null | null |
| 12 | CR-9981 | null | null | null |
| 14 | DC-8732 | null | null | null |

**Product Subcategory**

| ProductSubcategoryKey | Product Subcategory | ProductCategoryKey |
|---|---|---|
| 1 | Mountain Bikes | 1 |
| 2 | Road Bikes | 1 |
| 3 | Touring Bikes | 1 |
| 4 | Handlebars | 2 |
| 5 | Bottom Brackets | 2 |

**Join Kind**

Left Outer (all from first, matching from second)

Use fuzzy matching to perform the merge

Fuzzy matching options

OK

Cancel

Figure 5.43: Merging queries via the UI uses different Power Query functions, depending on the matching type

If we do not tick the Use fuzzy matching to perform the merge box, Power Query generates the following function:

```
Table.NestedJoin(
    FirstTable as table
  , KeyColumnofFirstTable as any
  , SecondTable as any
  , KeyColumnofSecondTable as any
  , NewColumnName as text
  , optional JoinKind as nullable JoinKind.Type
)
```

Otherwise, Power Query generates the following:
