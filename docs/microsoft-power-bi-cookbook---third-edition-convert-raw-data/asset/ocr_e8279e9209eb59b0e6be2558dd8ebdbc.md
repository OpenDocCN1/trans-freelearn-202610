# Getting ready

To prepare for this recipe, follow these steps:

+   1. Ensure that the user has a Power BI Pro license.
+   2. Confirm that the Power BI Pro user has access to the workspace containing the Power BI report and semantic model.
+   3. Choose a report to use for the analysis. For example, if you have completed the recipes in Chapter 5, Working in the Service, CountryMonthlyReport is a good choice. You can download the PBIX for this report from GitHub and publish it to the Power BI service.

You are now ready to complete this recipe.

# How to access and analyze Power BI semantic models in Excel

To implement this recipe, do the following:

+   1. Open the chosen report in the Power BI service.
+   2. In the header/ribbon for the report, choose Export and then Analyze in Excel.
+   3. Open the Excel file online, and then open the file in the Excel desktop application.
+   4. Click on the pivot table and add measures and columns from the semantic model:

![](bbox=[0.14, 0.46, 0.87, 0.75])

Figure 12.4: Excel pivot table with two slicers based on the Power BI semantic model

+   5. Select a cell within the pivot table.
+   6. Select the OLAP Tools dropdown from the PivotTable Analyze tab and click on Convert to Formulas: