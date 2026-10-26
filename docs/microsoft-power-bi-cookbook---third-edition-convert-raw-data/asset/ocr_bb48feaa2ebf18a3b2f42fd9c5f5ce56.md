Definitions of the available **Privacy Level** settings are provided in *Table 2.1.*

| Privacy Setting | Description |
|---|---|
| None | No privacy level defined. |
| Private | A **Private** data source is completely isolated from other data sources during query retrieval. For example, marking a text file **Private** would prevent that data from being processed on an external server. |
| Organizational | An **Organizational** data source is isolated from all public data sources but is visible to other organizational data sources during retrieval. |
| Public | A **Public** data source is visible to other sources. Only files, internet sources, and workbook data can be marked as **Public**. |

*Table 2.1: Privacy Level settings*

A single data source connection can be used by multiple queries. Any query that connects to the source system specified in the data source connection will use the cached connection information for that source system.

Once connected to the source system, the transformation steps specified in the **APPLIED STEPS** area of Power Query Editor are applied to the data over on the right side of the window. This transformed data is then imported into an Analysis Services tabular cube, hosted locally on the computer running Power BI Desktop. The tables imported into this Analysis Services cube are then displayed in Power BI Desktop in the Data pane.

To understand how the data is transformed while importing, click on **Transform data** in the **Queries** section of the ribbon on the **Home** tab. In Power Query Editor, select the Products query in the **Queries** pane. Next, select **Advanced Editor** in the **Query** section ribbon on the **Home** tab. The query should contain the following M code:

```
let
    Source = AdventureWorksDW2022,
    dbo_DimProduct = Source{[Schema = "dbo", Item = "DimProduct"]}[Data],
    #"Expanded DimProductSubcategory" = Table.ExpandRecordColumn(
        dbo_DimProduct,
        "DimProductSubcategory",
        {"EnglishProductSubcategoryName", "DimProductCategory"},
        {"EnglishProductSubcategoryName", "DimProductCategory"}
    ),
    #"Expanded DimProductCategory" = Table.ExpandRecordColumn(
        #"Expanded DimProductSubcategory",
        "DimProductCategory",
        {"EnglishProductCategoryName"},
        {"EnglishProductCategoryName"}
    ),
```