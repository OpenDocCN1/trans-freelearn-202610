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
[’2012_World_Pop$’].[Country] AS [Country], SUM([’2012_World_Pop$’].[Population])
AS [TEMP(Calculation_418553293854285824)(616435453)(0)]
FROM
[dbo].[’2012_World_Pop$’] [’2012_World_Pop$’]
WHERE
((’2012_World_Pop$’).[Country]= 'Afghanistan')
GROUP BY
[’2012_World_Pop$’].[Country] ) [t0]
CROSS JOIN
( SELECT SUM([’2012_World_Pop$’].[Population]) AS [ measure 0] FROM
[dbo].[’2012_World_Pop$’] [’2012_World_Pop$’]
GROUP BY () ) [t1]
```

Note the term CROSS JOIN; the LOD calculation generates a query that instructs the underlying data source engine to return data in such a way as to allow Tableau to divide the population values of one or more countries by the world population total, thus returning the correct percentage of the total.

# # Using the LOD INCLUDE calculation

Next, we will practice the INCLUDE LOD calculation. In this exercise, we will create a worksheet that displays the following:

+   - Total sales per region
+   - The average of total sales across all regions
+   - The average of total sales across all states in each region