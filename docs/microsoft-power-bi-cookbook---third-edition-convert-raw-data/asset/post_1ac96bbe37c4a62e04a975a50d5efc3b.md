
- 6. Power Query Editor launches in a new window with a query called DimGeography. Power Query Editor is a complementary tool included with Power BI Desktop and is used to connect to and transform data. The Power Query Editor window should look similar to Figure 2.2:

    ![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_66_0.png)

    Figure 2.2: Power Query Editor window

- 7. Observe the elements highlighted in Figure 2.2. On the left side is the Queries pane that contains the query we just created. In the center area, a preview of the data is displayed. The right-hand side, under APPLIED STEPS, displays the transformations we perform on the data. The highlighted block in Figure 2.2 just above the preview data displays the actual Power Query (M) code for the highlighted step. This is the formula bar.

    The formula bar is useful to quickly understand the M code behind a particular query step, exposing the M function for the current step only. This formula bar appears just above the column headers for the preview data in the central part of the window. If you do not see this formula bar, click the View tab and check the box next to Formula Bar in the Layout section of the ribbon.

    Every time you click on a button to connect to any of Power BI Desktop's supported data sources or apply any transformation to a data source object in Power Query Editor, such as navigating to a table in a data source or changing a column's data type, one or multiple M expressions are created, reflecting your choices. These M expressions are automatically written to dedicated M documents and, if saved, are stored within the Power BI Desktop file as queries. M is a functional programming language like F#, and it is important that Power BI developers become familiar with analyzing, understanding, and, later, writing and enhancing the M code that supports their queries.

- 8. Click the X icon next to the Navigation step under APPLIED STEPS. Notice that the preview data changes to become a list of the tables and views within the AdventureWorksDW2022 database. In addition, the formula bar switches to display:

    ```
    = Sql.Database("localhost", "AdventureWorksDW2022")
    ```

- 9. As mentioned, the formula bar only displays the M code for the current step in the query. To observe the entire M code for the query, click Advanced Editor just above the formula bar, as highlighted in Figure 2.2. The Advanced Editor dialog will appear, as shown in Figure 2.3:

    ![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_67_0.png)

    Figure 2.3: Advanced Editor dialog

- 10. The majority of queries created for Power BI follow the let . . . in structure, as shown in Figure 2.3. This will be addressed in greater detail later in this chapter.

- 11. The M function that is used is Sql.Database. This function accepts two parameters: the server name, localhost\MSSQLSERVERDEV, and the database name, AdventureWorksDW2022. Clicking on other steps under APPLIED STEPS exposes the formulas for those steps, which are technically individual M expressions.

- 12. Click the Done button to return to the Power Query Editor window.

- 13. Just above the APPLIED STEPS area is the PROPERTIES area. Use the Name field to change the name of the query to AdventureWorksDW2022.

This completes the preparation for this recipe.

## How to import data

To implement this recipe, perform the following steps:

- 1. In the Queries pane, right-click the AdventureWorksDW2022 query and choose Reference. This creates a new query that references the AdventureWorksDW2022 query as its source.
- 2. Select this new query, and in the preview data, find the DimProduct table in the Name column. Click on the Table link in the Data column for this row.
- 3. Rename this query Products.
- 4. Scroll to the right in the preview data area until you see the column DimProductSubcategory. Click the expand table icon to the right of the column name and uncheck all columns, except for EnglishProductSubcategoryName and DimProductCategory, as shown in Figure 2.4:

    ![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_68_0.png)

    Figure 2.4: Expand table dialog

- 5. As shown in Figure 2.4, uncheck the checkbox for Use original column name as prefix, and then click the OK button.

- 6. In the preview data area, you now have columns for EnglishProductSubcategoryName and DimProductCategory. Notice that a step has been added to the APPLIED STEPS area.

- 7. Click the expansion icon to the right of the DimProductCategory column and unselect all columns, except for EnglishProductCategoryName.

- 8. Ensure that the Use original column name as prefix box is unchecked, and then click the OK button. Notice that a step has been added to the APPLIED STEPS area.

- 9. Double-click the column header for EnglishProductCategoryName and rename this column Product Category. Notice that a Renamed Columns step has been added to the APPLIED STEPS area.

- 10. Repeat step 9 for EnglishProductSubcategoryName and EnglishProductName, renaming these columns to Product Subcategory and Product Name, respectively. Notice that, this time, an additional step is not added under APPLIED STEPS but, rather, the formula bar changes to reflect the renaming of these additional columns.

- 11. In the **Queries** pane, right-click the AdventureWorksDW2022 query and deselect **Enable load**. Notice that the query name is now italicized. Disabling the loading of queries prevents those queries from loading data into the data model.

- 12. Now, right-click the AdventureWorksDW2022 query again and select **Move To Group, New Group...**, as shown in *Figure 2.5*:

    ![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_69_0.png)

    Figure 2.5: Grouping queries

- 13. In the **New Group** dialog, type Data Sources for the name of the group, and then click the **OK** button.

Grouping queries helps you organize and maintain your queries. In complex data models, there may be dozens or even hundreds of individual queries, so it is important to maintain proper hygiene in order to manage your queries effectively.

- 14. Recall that we used the AdventureWorksDW2022 query as a reference for our Products query. We can observe this dependency by clicking on **Query Dependencies** in the **View** tab of the ribbon. This shows the dependency graphically, as shown in *Figure 2.6*.

To create this hierarchy, right-click the Product Category column in the Data pane and choose Create hierarchy. Rename this hierarchy Product Hierarchy. Next, right-click the Product Subcategory column and choose Add to hierarchy | Product Hierarchy. Repeat this step for the Product Name column.

## How it works

Importing data creates a data source connection that is cached on the local computer running Power BI Desktop. These data source connections can be seen by going to the File menu in the ribbon of either Power BI Desktop or Power Query Editor, choosing Options and settings, and then Data source settings. Doing this displays a dialog similar to Figure 2.8:

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_71_0.png)

Figure 2.8: Data source settings

As shown in Figure 2.8, the localhost;AdventureWorksDW2022 data source connection was selected, and then the Edit Permissions button was clicked. This allows the credentials to be edited or removed and also displays whether to encrypt the connection and its privacy level.

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

To understand how the data is transformed while importing, click on **Transform data** in the **Queries** section of the ribbon on the **Home** tab. In Power Query Editor, select the Products query in the **Queries** pane. Next, select **Advanced Editor** in the **Query** section of the ribbon on the **Home** tab. The query should contain the following M code:

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
