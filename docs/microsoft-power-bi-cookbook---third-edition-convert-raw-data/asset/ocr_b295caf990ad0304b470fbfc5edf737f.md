+   6. Click the **Start** button.
+   7. A migration completion message will appear that breaks down the different items completed:

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

+   8. Click the **Close** button.
+   9. Save the Power BI Desktop file and use the **Model view** to confirm that all relationships were imported successfully.
+   10. Click **Refresh** from the ribbon of the **Home** tab to test that all M queries were imported successfully.
+   11. With essential testing complete, click **Publish** from the ribbon of the **Home** tab and choose a workspace for the new semantic model.

Your new Power BI semantic model is now available in the Power BI service. You can even create new live connection reports against this semantic model if desired.

# How it works

The migration may take a few minutes, depending on the size of the semantic model imported. Power BI Desktop imports M queries, semantic model tables, DAX measures and KPIs, and any Power View for Excel sheets.

Workbooks with significant dependencies on items that do not import, such as Excel formulas, standard Excel tables (not model tables), worksheet range data, standard Excel charts, and conditional formatting, may need to remain supported in some capacity. For example, a minimum amount of data could be imported into Excel's semantic model to continue to drive Excel-based reports, and this workbook could be uploaded to the Power BI service and refreshed.