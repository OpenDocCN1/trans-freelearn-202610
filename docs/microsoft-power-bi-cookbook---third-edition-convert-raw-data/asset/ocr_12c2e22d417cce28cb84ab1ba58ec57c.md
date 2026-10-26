+   - Power Apps
+   - Power Automate
+   - ADF
+   - SSIS
+   - SQL Server Analysis Services
+   - Dynamics 365 Customer Insights

While both fast and powerful, there are times when you may find that a particular query is not as performant as desired. In these instances, Query Diagnostics can help you pinpoint problematic expressions and better understand what Power Query is doing in order to identify areas for query optimization. This recipe demonstrates how a user can use Query Diagnostics to troubleshoot a query and identify how the query might be optimized to be more performant.

# Getting ready

To get ready, import the FactCurrencyRate table from the AdventureWorksDW2022 database by doing the following:

+   1. Open Power BI Desktop and choose **Transform data** from the ribbon of the **Home** tab to open **Power Query Editor**.
+   2. Create an Import mode data source query called AdventureWorksDW2022. This query should be similar to the following:
+   3. Isolate this query in a query group called Data Sources by right-clicking the query in the Queries pane and selecting Move To Group | New Group.
+   4. Right-click the AdventureWorksDW2022 query and choose Reference.
+   5. Choose the FactCurrencyRate table link in the Data column and rename the query Currency Rates.
+   6. In **Power Query Editor**, select the **Tools** tab and then **Diagnostic Options**.

```
```text
let
  Source = Sql.Database("localhost", "AdventureWorksDW2022")
in
  Source
```
```