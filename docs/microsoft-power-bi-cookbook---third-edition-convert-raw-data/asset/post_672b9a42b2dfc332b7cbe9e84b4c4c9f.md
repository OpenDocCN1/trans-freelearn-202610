
6. In Teams, select the ellipsis (...) from the navigation pane and search for power bi.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_505_0.png)

Figure 11.41: Adding the Power BI app in Teams

7. Select the Power BI app.

8. The Power BI app is now pinned to the Teams navigation bar.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_505_1.png)

Figure 11.42: The Power BI app in Teams

As you can see, there are many integration points between Power BI and Teams.

## See also

- Collaborate with Power BI in Microsoft Teams, Outlook, and Office: https://bit.ly/2QYrXg1
- Add the Power BI app to Microsoft Teams: https://bit.ly/2RD81y8

## Summary

This chapter contained detailed examples and considerations for deploying and distributing Power BI content via the Power BI service. This included the creation and configuration of app workspaces and apps, configuring data gateways, configuring data sources and refresh schedules, and best practices for workspaces. Additionally, processes and sample architectures were shared, describing staged deployments across development and production environments.

In the next chapter, we will explore integrating Power BI with other applications, including SQL Server Reporting Services, Excel, PowerPoint, Azure Analysis Services, Dataverse, Dynamics 365, and more!

## Join our community on Discord

Join our community's Discord space for discussions with the author and other readers:

https://discord.gg/ebXZqyrxJb

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_506_0.png)

## 12

## Integrating Power BI with Other Applications

Power BI tools and services, including Power BI Desktop, the Power BI web service, and Power BI mobile applications, form a modern, robust business intelligence and analytics platform by themselves. Power BI Premium and Microsoft Fabric capacities further extend the scalability and deployment options of Power BI solutions, enabling organizations to deliver Power BI content to large groups of users via apps in the Power BI service, embedded within custom applications, the on-premises Power BI Report Server, or some combination of these distribution options.

However, many organizations either already have extensive self-service and corporate BI assets and skills in other tools, such as Excel, Analysis Services, and Reporting Services, or are interested in utilizing the unique features of these tools as part of their Power BI solutions. As one example, an organization may choose to keep many existing Excel reports in Excel rather than convert them to Power BI reports, given the unique features of Excel such as cell-level data, formatting control, and cube formulas. Alternatively, given the many additional features and benefits of Power BI semantic models, organizations with legacy Excel-based data models may choose to migrate these assets to Power BI.

The recipes in this chapter highlight powerful integration points between Power BI, Excel, PowerPoint, Power Apps, Power Automate, Dataverse, and Dynamics 365. In this chapter, we will cover the following recipes:

- Migrating from Power Pivot for Excel to Power BI
- Accessing and analyzing Power BI semantic models in Excel
- Building Power BI reports into PowerPoint Presentations
- Integrating with Power Automate and Power Apps
- Leveraging Dataverse and Dynamics 365
- Connecting Dynamics 365 and Power Platform

At the end of this chapter, you will have a firm understanding of how Power BI works and integrates with other Microsoft technologies.

## Technical Requirements

The following are required to complete the recipes in this chapter:

- Power BI Desktop.
- Excel.
- PowerPoint.
- Visual Studio.
- Power Apps.
- Power Automate Premium.
- Dynamics 365.
- SQL Server 2022 or newer with the AdventureWorksDW2022 database installed. See the Installing SQL Server and the AdventureWorks Database recipe from Chapter 1, Installing and Licensing Power BI Tools.
- Files for this chapter can be downloaded from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition/.

## Migrating from Power Pivot for Excel to Power BI

Given the maturity of Power BI as a platform and the increased comfort level with Power BI Desktop among users, it's often beneficial to migrate semantic models (formerly Power Pivot) and M queries from Excel to Power BI. From a data management and governance standpoint, it is preferable to consolidate semantic models to either Power BI and/or Analysis Services models and to limit Excel's role to ad hoc analysis, such as pivot tables connected to semantic models in the Power BI service via Analyze in Excel.

In this recipe, a semantic model and its source M queries contained in an Excel workbook are migrated to a Power BI semantic model, via the Import Excel Workbook to Power BI Desktop migration feature. Additional details on the workbook content imported, and other options and considerations for Excel-to-Power BI migrations, are included in the How it works and There's more... sections.

## Getting ready

Analyze the Excel workbook to identify the components that can be imported to Power BI Desktop. For example, a table or range of data in an Excel worksheet will not be imported, but tables in the Excel semantic model will be imported. Similarly, Power View report sheets in Excel and their visuals will be migrated, but standard Excel charts, pivot tables, and worksheet formulas and formatting will not be migrated.

In some scenarios, it may be necessary to revise the Excel workbook to establish a data source connection and query that will be migrated. Additionally, it may be necessary to recreate Excel-specific report visualizations, such as pivot tables and charts, with Power BI Desktop report authoring visuals. Excel workbooks that contain a high level of customization, such as VBA macros and complex Excel formula logic, may require significant modifications to the Excel workbook, the Power BI Desktop model, or some combination of both to support migration.

- 6. Click the **Start** button.
- 7. A migration completion message will appear that breaks down the different items completed:

# Import Excel workbook contents

Migration completed

Queries (5 items)
✓ DimAccount
✓ DimCurrency
✓ DimCustomer
✓ DimDate
✓ FactInternetSales

Data model tables (5 items)
✓ DimAccount

Figure 12.3: Import Excel Model to Power BI Desktop migration completed

- 8. Click the **Close** button.
- 9. Save the Power BI Desktop file and use the **Model view** to confirm that all relationships were imported successfully.
- 10. Click **Refresh** from the ribbon of the **Home** tab to test that all M queries were imported successfully.
- 11. With essential testing complete, click **Publish** from the ribbon of the **Home** tab and choose a workspace for the new semantic model.

Your new Power BI semantic model is now available in the Power BI service. You can even create new live connection reports against this semantic model if desired.

## How it works

The migration may take a few minutes, depending on the size of the semantic model imported. Power BI Desktop imports M queries, semantic model tables, DAX measures and KPIs, and any Power View for Excel sheets.

Workbooks with significant dependencies on items that do not import, such as Excel formulas, standard Excel tables (not model tables), worksheet range data, standard Excel charts, and conditional formatting, may need to remain supported in some capacity. For example, a minimum amount of data could be imported into Excel's semantic model to continue to drive Excel-based reports, and this workbook could be uploaded to the Power BI service and refreshed.

## Getting ready

To prepare for this recipe, follow these steps:

- 1. Ensure that the user has a Power BI Pro license.
- 2. Confirm that the Power BI Pro user has access to the workspace containing the Power BI report and semantic model.
- 3. Choose a report to use for the analysis. For example, if you have completed the recipes in Chapter 5, Working in the Service, CountryMonthlyReport is a good choice. You can download the PBIX for this report from GitHub and publish it to the Power BI service.

You are now ready to complete this recipe.

## How to access and analyze Power BI semantic models in Excel

To implement this recipe, do the following:

- 1. Open the chosen report in the Power BI service.
- 2. In the header/ribbon for the report, choose Export and then Analyze in Excel.
- 3. Open the Excel file online, and then open the file in the Excel desktop application.
- 4. Click on the pivot table and add measures and columns from the semantic model:

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_512_0.png)

Figure 12.4: Excel pivot table with two slicers based on the Power BI semantic model

- 5. Select a cell within the pivot table.
- 6. Select the OLAP Tools dropdown from the PivotTable Analyze tab and click on Convert to Formulas:
