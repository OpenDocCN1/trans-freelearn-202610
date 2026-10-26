9. As mentioned, the formula bar only displays the M code for the current step in the query. To observe the entire M code for the query, click Advanced Editor just above the formula bar, as highlighted in Figure 2.2. The Advanced Editor dialog will appear, as shown in Figure 2.3:

![](bbox=[0.19, 0.14, 0.89, 0.4])

Figure 2.3: Advanced Editor dialog

10. The majority of queries created for Power BI follow the let . . . in structure, as shown in Figure 2.3. This will be addressed in greater detail later in this chapter.

11. The M function that is used is Sql.Database. This function accepts two parameters: the server name, localhost\MSSQLSERVERDEV, and the database name, AdventureWorksDW2019. Clicking on other steps under APPLIED STEPS exposes the formulas for those steps, which are technically individual M expressions.

12. Click the Done button to return to the Power Query Editor window.

13. Just above the APPLIED STEPS area is the PROPERTIES area. Use the Name field to change the name of the query to AdventureWorksDW2022.

This completes the preparation for this recipe.

# How to import data

To implement this recipe, perform the following steps:

+   1. In the Queries pane, right-click the AdventureWorksDW2022 query and choose Reference. This creates a new query that references the AdventureWorksDW2022 query as its source.
+   2. Select this new query, and in the preview data, find the DimProduct table in the Name column. Click on the Table link in the Data column for this row.
+   3. Rename this query Products.