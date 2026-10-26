
- Power Apps
- Power Automate
- ADF
- SSIS
- SQL Server Analysis Services
- Dynamics 365 Customer Insights

While both fast and powerful, there are times when you may find that a particular query is not as performant as desired. In these instances, Query Diagnostics can help you pinpoint problematic expressions and better understand what Power Query is doing in order to identify areas for query optimization. This recipe demonstrates how a user can use Query Diagnostics to troubleshoot a query and identify how the query might be optimized to be more performant.

## Getting ready

To get ready, import the FactCurrencyRate table from the AdventureWorksDW2022 database by doing the following:

- 1. Open Power BI Desktop and choose **Transform data** from the ribbon of the **Home** tab to open **Power Query Editor**.
- 2. Create an Import mode data source query called AdventureWorksDW2022. This query should be similar to the following:
- 3. Isolate this query in a query group called Data Sources by right-clicking the query in the Queries pane and selecting Move To Group | New Group.
- 4. Right-click the AdventureWorksDW2022 query and choose Reference.
- 5. Choose the FactCurrencyRate table link in the Data column and rename the query Currency Rates.
- 6. In **Power Query Editor**, select the **Tools** tab and then **Diagnostic Options**.

```text
let
  Source = Sql.Database("localhost", "AdventureWorksDW2022")
in
  Source
```

- 7. Ensure that the radio button for **Enable in Query Editor (does not require running as admin)** is selected, and check all the boxes under the **Diagnostics Level** and **Additional Diagnostics** sub-headings.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_122_0.png)

*Figure 2.39: Query Diagnostics options*

For additional details on performing these steps, see the *Importing Data* recipe in this chapter.

## How to diagnose queries

To implement this recipe, perform the following steps:

- 1. Select the Currency Rates query, open **Advanced Editor**, and modify the query to calculate the daily change in currency rates.

```powerquery
let
    Source = AdventureWorksDW2022,
    dbo_FactCurrencyRate = Source{[Schema="dbo",Item="FactCurrencyRate"]}[Data],
    Sort = Table.Sort(
        dbo_FactCurrencyRate,
        {{"CurrencyKey", Order.Ascending}, {"DateKey", Order.Ascending}}
    ),
    Index = Table.TransformColumnTypes(
        Table.AddIndexColumn(Sort, "Row Index", 1, 1),
        {{"Row Index", Int64.Type}}
    ),
```

```
PrevIndex = Table.TransformColumnTypes(
    Table.AddIndexColumn(Index, "Prev Index", 0, 1),
    {{"Prev Index", Int64.Type}}
),
SelfJoin = Table.NestedJoin(
    PrevIndex, {"Prev Index"}, PrevIndex, {"Row Index"},
    "NewColumn", JoinKind.LeftOuter
),
PrevColumns = Table.ExpandTableColumn(
    SelfJoin, "NewColumn",
    {"EndOfDayRate", "CurrencyKey"}, {"PrevRate", "PrevKey"}
),
AddChange = Table.AddColumn(
    PrevColumns, "Daily Change", each
    if [CurrencyKey] = [PrevKey] then [EndOfDayRate] - [PrevRate]
    else null, type number
),
SelectColumns = Table.SelectColumns(
    AddChange,
    {"CurrencyKey", "EndOfDayRate", "Date", "Daily Change"}
)
in
SelectColumns
```

- 2. You may have to wait several minutes for the preview data to display. To investigate what is going on, click on the Tools tab and then Start Diagnostics.
- 3. Click back on the Home tab and click Refresh Preview.
- 4. Wait for the preview data to refresh, and then click back on the Tools tab and Stop Diagnostics.

## Diagnostics

Please wait while we summarize the traces.

Figure 2.40: Diagnostics processing display

5. When the diagnostics processing completes, a new query group called Diagnostics is created containing four queries, Diagnostics_Counters, Diagnostic_Detailed, Diagnostic_Aggregated, and Diagnostic_Partitions.

Each of these queries is suffixed with a date and time stamp of when the diagnostics were run, and each has loading disabled.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_124_0.png)

Figure 2.41: Diagnostics queries

6. Click on the Diagnostics_Aggregated query. Select the Step, Category, Start Time, End Time, Exclusive Duration (%), and Exclusive Duration columns and remove the other columns.
7. Sort the Exclusive Duration column in descending order.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_124_1.png)

Figure 2.42: Diagnostic results

8. Look for large jumps in the Exclusive Duration column; we see two such jumps between lines 3 and 4, and 5 and 6, with the larger jump being between lines 3 and 4. Also, note the high Exclusive Duration (%) value for row 3.
9. Look at the Start Time and End Time columns for row 3, and note that this operation took 2 minutes and 9 seconds to complete. The Step column shows that the query step for row 3 is the PrevColumns step.
10. Having zeroed in on the PrevColumns step of the query, click on the Diagnostics_Detailed query and filter the Step column to just show PrevColumns. If the PrevColumns value does not appear in the filtering dialog, use the Load More link or edit the query in Advanced Editor to add the Table.SelectRows expression manually: Table.SelectRows(#"Changed Type", each [Step] = "PrevColumns").
11. Sort the query by the Exclusive Duration (%) column in descending order.
12. Note that the Operation column for the top row is DbDataReader.

At this point, it should be evident that this query would likely be better done in the source database system as a view or stored procedure.

## How it works

The Currency Rates query joins the base table with itself in order to compare subsequent rows of data. The goal is to compare the values of one row with the previous row in order to compute a value for a change in currency rate between days. This is done by sorting the table by first the currency and then the date, using the Table.Sort function.

Next, the Table.AddIndexColumn function is used twice, once to add an index column starting from 1 (the Row Index column) and a second time to add an index column starting from 0 (the Prev Index column). These two index columns are then used in a Table.NestedJoin function to join the table to itself.

The Table.ExpandTableColumn function is used to expose the previous row’s CurrencyKey and EndOfDayRate columns. This information can then be used to create the Daily Change column, using the Table.AddColumns function, and finally, only the essential columns are selected, using the Table.SelectColumns function.

When you start Query Diagnostics, query diagnostic information is logged to the JSON and CSV files stored in the application’s directory. These files can be located by looking at the Source step for aggregated, detailed, and partition diagnostic queries and the CsvFiles step of the Counters diagnostic query. For the Power BI Desktop Store App, the path should be similar to the following:

```
C:\Users\[user]\Microsoft\Power BI Desktop Store App\Traces\Diagnostics
```

These files record diagnostic information performed by the query engine during processes such as refreshing the preview data. Stopping Query Diagnostics ends logging of the diagnostic data and generates queries for each enabled Query Diagnostics feature: Aggregated, Detailed, Performance counters, and Data privacy partitions. As diagnostic logging is costly in terms of performance and system resources, it is recommended to only use Query Diagnostics when troubleshooting a query’s performance. In addition, only enable the minimal amount of diagnostic logging required to identify the problem—for example, often just starting with the Aggregated diagnostic data is enough to identify the problematic step(s).

## There’s more…

There is also a Diagnose Step feature available for Query Diagnostics. To see how Diagnose Step can be used, follow these steps:

- 1. Open Diagnostic Options from the ribbon of the Tools tab and uncheck the Performance counters and Data privacy partitions.
- 2. In the Query Settings pane, select the SelfJoin step. You can now either right-click the SelfJoin step and select Diagnose or select Diagnose Step in the ribbon of the Tools tab.
- 3. Once complete, two additional queries are added to the Diagnostics query group, Currency Rates_SelfJoin_Detailed and Currency Rates_SelfJoin_Aggregated, each suffixed with a date and time stamp.
- 4. Click on the Currency Rates_SelfJoin_Aggregated query.
- 5. Add an Index column, starting at 1.
