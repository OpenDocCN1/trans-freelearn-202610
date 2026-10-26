# Getting ready

To prepare for this recipe, follow these steps:

+   1. Download Building Dynamic Security for DirectQuery Start.pbix from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition.
+   2. Open Building Dynamic Security for DirectQuery Start.pbix in Power BI Desktop. You may receive an error regarding being able to connect to the data source.
+   3. Click Transform data from the ribbon of the Home tab, and edit the Source step of each of the four queries to point to your SQL server with the AdventureWorksDW2022 database installed.
+   4. Select Close & Apply from the ribbon of the Home tab.
+   5. Save your work.

# How to build dynamic security models for DirectQuery

To implement this recipe, perform the following:

+   1. In the Model view, open the properties for the Countries to Employees relationship, and set the Cross filter direction to Both.
+   2. Check the checkboxes for both Assume referential integrity and Apply security filter in both directions, and then click OK.

![](bbox=[0.2, 0.47, 0.9, 0.57])

Figure 8.12: Bidirectional relationship between the Countries and Employees tables

+   3. The model should now look the same as that shown in Figure 8.13: