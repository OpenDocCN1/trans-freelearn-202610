
![](img/66cf6b89349fda3581a4f196cb3c98a4_311_0.png)

Figure 7.35: Format Reference Line

6. In Tableau Desktop, you should now see the following. As before, both **Reference Lines** are equal:

![](img/66cf6b89349fda3581a4f196cb3c98a4_311_1.png)

Figure 7.36: Reference Lines

7. Change the **Choose Included Dims** parameter to **Category, Ship Mode, and Segment**

8. Note that the **Reference Lines** equal one another for each of these settings. This is because **Choose Included Dims** is only introducing dimensions already represented in the view.

9. Change the **Choose Included Dims** parameter to **Sub-Category** to see the additional reference line in its intended place:

![](img/66cf6b89349fda3581a4f196cb3c98a4_312_0.png)

Figure 7.37: Showing an additional reference line

The **Include Dim Avg** reference line now includes **Sub-Category** in the average and therefore differs from the **Overall Avg** reference line. The average is smaller because the total **Sales** amount is being divided over more points than are visible in the view. Adding **Sub-Category** to the LOD leads to more rows, which leads to a smaller average per row. Furthermore, note that the **Sub-Category** dimension is not used in the view. LODs do not require a calculation to reside in the view.

To summarize, the **INCLUDE** LOD can manipulate a visualization in a way that partitions can be added that are not used in the view itself. Also, the naming convention for all three LODs will be your best mnemonic in understanding them: Fixed to fix the fields used, Include to add “missing” fields, and Exclude to remove unwanted fields from the calculations that are already present.

To get more hands-on training, we will continue with a few practical applications.

## Building practical applications with LODs

The first portion of this chapter was designed to demonstrate how LODs work. The remainder will be dedicated to practical applications. Specifically, we will consider three typical challenges that previously were solved using other Tableau capabilities, such as table calculations and data blending.

This exercise will look at a problem that occurs when mixing a table calculation that calculates the percentage of the total with a dimension filter. We will consider the problem, a solution using an LOD calculation, and finish with a commentary section on the germane points of the solution.

### Using the LOD FIXED calculation

Did you ever encounter a situation where you wanted to calculate the Percent of Total while only showing a subset of the Total dimension in your view? Well, you are not alone. But thanks to LODs, you will soon know how to solve this problem. The following steps will guide you through the exercise:

- 1. Open the workbook associated with this chapter and navigate to the worksheet entitled The Problem
- 2. Select the 2012_World_Pop dataset

- 3. Create a calculated field named Percent of Total with the following code:

```
SUM([Population]) / TOTAL(SUM([Population]))
```

- 4. Right-click on Percent of Total and select Default Properties | Number Format | Percentage
- 5. Place Country on the Columns shelf, Measure Names on the Rows shelf, and Measure Values on the Text shelf
- 6. Remove Number of Records from the Measure Values shelf
- 7. Right-click on Percent of Total and set it to Compute Using | Country

- 8. Create a filter on Country such that only Afghanistan displays:

![](img/66cf6b89349fda3581a4f196cb3c98a4_313_0.png)

Figure 7.38: Population of Afghanistan

Afghanistan displays the percentage of the total as 100%. Obviously, this does not represent Afghanistan’s percentage of the total population of the world. Let’s investigate a possible solution—to solve the issue with an LOD, follow along with the exercise steps:

- 9. Duplicate the worksheet entitled The Problem and rename it Solution LOD

- 10. Create a calculated field named Percent of Total LOD with the following code:

```
SUM([Population])/SUM({ FIXED : SUM([Population])})
```

- 11. Place Percent of Total LOD on the Measure Values shelf and note that the displayed percentage equals 0.43%. If 0 displays, change the number formatting to percentage:

![](img/66cf6b89349fda3581a4f196cb3c98a4_314_0.png)

Figure 7.39: Percent of total

- 12. Remove the Percent of Total table calculation that was previously created from the Measure Values shelf:

| Country | Afghanistan |
| :--- | :--- |
| Population | 29,824,536 |
| Percent of Total LOD | 0.43% |

Figure 7.40: Percent of total LOD

As you can see, problem solved! To better understand what is happening in the background, let’s look at a query that Tableau generates for the worksheet entitled The Problem. Generated queries can be viewed by clicking **Help | Settings and Performance | Start Performance Recording**. (See Chapter 11, Designing Dashboards and Best Practices for Visualizations, for additional information regarding viewing Tableau’s generated queries.)

The SQL statement is as follows:

```
SELECT
['2012_World_Pop$'].[Country] AS [Country],
SUM(['2012_World_Pop$'].[Population]) AS [SUM:Population:ok]
FROM
[dbo].['2012_World_Pop$'] ['2012_World_Pop$']
WHERE
(['2012_World_Pop$'].[Country] = 'Afghanistan')
GROUP BY
['2012_World_Pop$'].[Country]]
```

Note that the WHERE clause in the query is limiting the returned dataset to only those rows that have Afghanistan as **Country**. This WHERE clause was generated because of placing **Country** on the **Filters** shelf. By limiting the returned data, the processing requirements are shifted from Tableau to the underlying data source engine. In other words, the data source engine does the work of executing the query, and Tableau thus works with a smaller dataset. The reasoning behind this design is that data source engines are specifically engineered to efficiently query large datasets and typically have underlying hardware to support such activities. Furthermore, limiting the rows of data returned to Tableau can reduce inefficiencies due to latency.

Inefficiencies are further reduced because Tableau works from the cache whenever possible. Often, a user can perform various operations without generating a call to the underlying data source. However, in the preceding case, if a user were to select a different **Country** from the filter, Tableau would generate a new query to the data source. For example, if a user interacts with the filter and deselects Afghanistan and selects Albania, a new query with a corresponding WHERE clause is generated. Although the logic of Tableau's reliance on the data source engine is demonstrable, the problem proposed in the preceding example remains. What can a Tableau author do to calculate the percent of the whole regardless of filtering?

A solution is to create a table calculation that can also be used as a filter. The difference is that unlike in the worksheet The Problem, the query generated by Tableau to use as a data source returns the entire dataset. When a table calculation is used in a filter, the filtering does not take place until the underlying data is returned. In other words, Tableau performs the filtering. This preserves a percent of the total percentages regardless of which country population totals the user chooses to view. But a potential issue with table calculations is that the returned dataset may be quite large, which may cause performance challenges. Latency may be experienced due to the increased time required to return a large dataset, and Tableau may perform more slowly because of additional processing responsibilities to filter the dataset.

Our proposed solution, an LOD calculation, can address these challenges. Let's look at the SQL queries created by the Percent of Total LOD calculated field in the Solution LOD worksheet:

```
SELECT
[t0].[Country] AS [Country],
[t1].[ measure 0] AS
[TEMP(Calculation_418553293854285824)(2417030171)(0)],
[t0].[TEMP(Calculation_418553293854285824)(616435453)(0)] AS
[TEMP(Calculation_418553293854285824)(616435453)(0)]
FROM
( SELECT
['2012_World_Pop$'].[Country] AS [Country], SUM(['2012_World_Pop$'].[Population])
AS [TEMP(Calculation_418553293854285824)(616435453)(0)]
FROM
[dbo].['2012_World_Pop$'] ['2012_World_Pop$']
WHERE
(['2012_World_Pop$'].[Country]= 'Afghanistan')
GROUP BY
['2012_World_Pop$'].[Country] ) [t0]
CROSS JOIN
( SELECT SUM(['2012_World_Pop$'].[Population]) AS [ measure 0] FROM
[dbo].['2012_World_Pop$'] ['2012_World_Pop$']
GROUP BY () ) [t1]
```

Note the term CROSS JOIN; the LOD calculation generates a query that instructs the underlying data source engine to return data in such a way as to allow Tableau to divide the population values of one or more countries by the world population total, thus returning the correct percentage of the total.

### Using the LOD INCLUDE calculation

Next, we will practice the INCLUDE LOD calculation. In this exercise, we will create a worksheet that displays the following:

- Total sales per region
- The average of total sales across all regions
- The average of total sales across all states in each region
