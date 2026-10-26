# Getting ready

To prepare for this recipe, follow these steps:

+   1. In the Power BI service, click on Settings (the gear icon) in the top right, choose General, click Item settings, and enable ArcGIS maps. This requires administrative access to the Power BI service.

![](bbox=[0.188, 0.197, 0.915, 0.373])

+   2. Download Creating Geospatial Mapping with ArcGIS Maps Start.pbix from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition.

+   3. Open Creating Geospatial Mapping with ArcGIS Maps Start.pbix in Power BI Desktop.

+   4. Edit the AdventureWorks query, and change the Source line to point to your SQL server with the AdventureWorksDW2022 database installed.

+   5. Click Close & Apply from the ribbon of the Home tab.

# How to create geospatial mapping with ArcGIS maps

To implement this recipe, carry out the following steps:

+   1. In Report view, select the City column in the Customers table and assign a Data category of City.

+   2. Select the Full Address column in the Customers table and assign a Data category of Address.

+   3. Apply a page-level filter using the SalesTerritoryRegion column in the Customers table and select Southwest only.

+   4. In the Visualizations pane, select the ArcGIS Maps for Power BI visualization, and then expand the visualization's size to the entire page.

+   5. Click the Continue as guest button in the visual.

+   6. From the Customers table, add the Full Address column for Location, the CustomerKey column for Size, and the YearlyIncome column for Color.