
![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_446_0.png)

Figure 10.9: Performance analyzer results for last 7 days range measures

While neither Last 7 Days 1 (1) nor Last 7 Days 2 (2) are particularly efficient, the Last 7 Days 2 measure consistently performs approximately 25–30% faster than the Last 7 Days 1 measure. This is because the Last 7 Days 1 measure must evaluate each row of the Internet Sales table (about 60,000 rows) with a complex filter condition (&&) while the Last 7 Days 2 measure uses simpler filter conditions in sequence and reduces the number of rows required for analysis by the second simple filter condition. The Last 7 Days 3 measure takes this a step further and uses SUMMARIZE to immediately reduce the number of rows down to just the number of rows in the Dates table (about 3,600 rows), and this reduced table is then filtered by the subsequent simple filter conditions.

Note that the Last 7 Days 3 measure performs no better or worse than a traditional CALCULATE approach:

```
Last 7 Days (CALCULATE) =
    CALCULATE(
        [Internet Sales],
        'Dates'[Date] <= MAX('Dates'[Date]) && 'Dates'[Date] >= 
```
```
MAX('Dates'[Date]) - 6
)
```

At a high level, always think about the size of the table being filtered and look for simple filter conditions and single columns that can be used to reduce the size of this table. More complex expressions that cannot be handled by the storage engine can then operate against this smaller table. Similarly, consider (and test) nesting or tiering filter conditions such that the most selective, efficient filter condition is applied first. This approach can significantly improve the speeds of measures that utilize DAX’s SWITCH function by placing the conditions most likely to evaluate to TRUE at the top of the SWITCH statement.

## See also

- Use Performance Analyzer to examine report element performance in Power BI Desktop: https://bit.ly/3eWXdEb
- The Case for No CALCULATE: https://www.linkedin.com/pulse/case-calculate-greg-deckler/

## Pushing Query Processing Back to Source Systems

During the scheduled refresh of semantic models retrieving from on-premises sources, any query transformations not executed by the source system will require local resources of the M (Mashup) engine of the on-premises data gateway server. With larger semantic models, and potentially with other scheduled refreshes occurring on the same gateway server at the same time, it becomes important to design M queries that take full advantage of source system resources via query folding. Although transformations against some sources such as files will always require local resources, in many scenarios, M queries can be modified to help the engine generate an equivalent SQL or other source system statement and thus minimize local resource consumption.

In this recipe, a process and list of items are provided to identify queries not currently folding and the potential causes. Additionally, a query based on an existing SQL statement is redesigned with M expressions to allow query folding.

## Getting ready

To prepare for this recipe, follow these steps:

- 1. Identify the semantic model to evaluate for query folding.
- 2. Use gateway server memory performance counter data to establish a baseline of the resources currently used to perform refreshes as these counters should be impacted by any changes.

Also, establish a baseline for query performance using the new Diagnostic tools within Power Query Editor’s Tools tab.

The semantic model to be optimized will generally be a large PBIX file (100 MB+) published to the Power BI Service with a scheduled refresh configured to use an on-premises data gateway that queries a relational database (SQL or Oracle) as the primary source.

As an example of performing this process, consider the following case, where a business analyst has used a SQL statement and the Query Editor to construct a customer query:

```
let
    Source = Sql.Database("localhost", "AdventureWorksDW2022", {Query="SELECT * FROM dbo.DimCustomer"})
    #"Replaced Value" = Table.ReplaceValue(Source,"M","Married",Replacer.ReplaceText,{"MaritalStatus"}),
    #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value","S","Single",Replacer.ReplaceText,{"MaritalStatus"}),
    #"Filtered Rows" = Table.SelectRows(#"Replaced Value1", each true),
    #"Replaced Value2" = Table.ReplaceValue(#"Filtered Rows","M","Male",Replacer.ReplaceText,{"Gender"}),
    #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","F","Female",Replacer.ReplaceText,{"Gender"}),
    #"Filtered Rows1" = Table.SelectRows(#"Replaced Value3", each true),
    #"Renamed Columns" = Table.RenameColumns(#"Filtered Rows1",{{"EnglishEducation", "Education"}, {"EnglishOccupation", "Occupation"}}),
    #"Removed Columns" = Table.RemoveColumns(#"Renamed Columns",{"SpanishEducation", "FrenchEducation", "SpanishOccupation", "FrenchOccupation"}),
    // Customer History Date Bands
    CurrentDate = #date(2014,2,9),
    OneYearAgo = Date.AddYears(CurrentDate,-1),
    TwoYearsAgo = Date.AddYears(CurrentDate,-2),
    ThreeYearsAgo = Date.AddYears(CurrentDate,-3),
    //Customer Dimension
    CustomerHistoryColumn = Table.AddColumn(#"Removed Columns", "Customer History Segment", each
        if [DateFirstPurchase] >= OneYearAgo then "First Year Customer"
        else if [DateFirstPurchase] >= TwoYearsAgo and [DateFirstPurchase] < OneYearAgo then "Second Year Customer"
        else if [DateFirstPurchase] >= ThreeYearsAgo and [DateFirstPurchase] < TwoYearsAgo then "Third Year Customer"
        else "Legacy Customer", type text)
in
    CustomerHistoryColumn
```

Figure 10.11: Customer query based on native SQL statement and M transformations

In this scenario, the SQL statement is against the base customer table in the data warehouse (not the view) and the subsequent transformations applied against the query results all use local gateway server resources during each refresh process since the first step executes a native SQL query. The query can be modified to perform better by taking advantage of query folding as follows:

- 1. Create a new M query that uses parameters for the server and database and that uses the customer SQL view:

```
let
    Source = Sql.Database("localhost", "AdventureWorksDW2022"),
    dbo_Customer = Source[{Schema="dbo",Item="DimCustomer"}][Data],
    #"Replaced Value" = Table.ReplaceValue(dbo_Customer,"M","Married",Replacer.ReplaceText,{"MaritalStatus"}),
    #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value","S","Single",Replacer.ReplaceText,{"MaritalStatus"}),
    #"Filtered Rows" = Table.SelectRows(#"Replaced Value1", each true),
    #"Replaced Value2" = Table.ReplaceValue(#"Filtered Rows","M","Male",Replacer.ReplaceText,{"Gender"}),
    #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","F","Female",Replacer.ReplaceText,{"Gender"}),
    #"Filtered Rows1" = Table.SelectRows(#"Replaced Value3", each true),
    #"Renamed Columns" = Table.RenameColumns(#"Filtered Rows1",{{"EnglishEducation", "Education"}, {"EnglishOccupation", "Occupation"}}),
```
```
#"Removed Columns" = Table.RemoveColumns(#"Renamed Columns",{"SpanishEducation", "FrenchEducation", "SpanishOccupation", "FrenchOccupation"}),
    // Customer History Date Bands
    CurrentDate = #date(2014,2,9),
    OneYearAgo = Date.AddYears(CurrentDate,-1),
    TwoYearsAgo = Date.AddYears(CurrentDate,-2),
    ThreeYearsAgo = Date.AddYears(CurrentDate,-3),
    //Customer Dimension
    CustomerHistoryColumn = Table.AddColumn(#"Removed Columns", "Customer History Segment", each
        if [DateFirstPurchase] >= OneYearAgo then "First Year Customer"
        else if [DateFirstPurchase] >= TwoYearsAgo and [DateFirstPurchase] < OneYearAgo then "Second Year Customer"
        else if [DateFirstPurchase] >= ThreeYearsAgo and [DateFirstPurchase] < TwoYearsAgo then "Third Year Customer"
        else "Legacy Customer", type text)
in
    CustomerHistoryColumn
```

Native M functions are used to access the DimCustomer table instead of a SQL query statement and the entire query now folds back to SQL Server.

- 2. Right-click on the final step of the new, revised query and ensure that View Native Query is enabled.
