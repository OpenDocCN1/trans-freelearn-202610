# # Aggregation

The following exercise will use two aggregates from a single data source, one at the **State** level and the other at the **City** level. These aggregated data sources will be used to create two worksheets. Each of these worksheets will be placed on a dashboard along with a third worksheet with row-level information. Finally, filter actions will be created to tie the three worksheets together. The purpose of the exercise is to demonstrate how small extracts might be used in conjunction with a larger dataset to create a more performant dashboard:

+   1. Open the workbook associated with this chapter and navigate to the State Agg worksheet

+   2. In the **Data** pane, select the State Agg data source

+   3. Create a filled map using state by placing **State** on the **Detail** shelf and selecting **Filled Map** from the **Marks** card

+   4. Right-click on the State Agg data source and select **Extract Data**

+   5. Note that Tableau displays an error stating that it cannot find the referenced file. You can either point to the instance of **Superstore** that ships with Tableau or you can use the instance provided via the GitHub link: https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter12.

+   6. After connecting to the data source, Tableau will display the **Extract Data** dialog box. Within the dialog box, select **Aggregate data for visible dimensions** and **All rows**. Click the **Hide All Unused Fields** button and then click on **Extract**. Note that the resulting extract only contains **State**. Also note that the data has been aggregated so that no underlying data is available.

+   7. Navigate to the City Agg worksheet

+   8. In the **Data** pane, select the City Agg data source. Note that this data source has already been extracted and so only contains **State**, **City**, and **Sales**. Also note that the data has been aggregated so that no underlying data is available.

+   9. Place **City** on the **Rows** shelf, **Sales** on the **Text** shelf, and **State** on the **Detail** shelf. Don't forget to include **State** even though it does not display on the view. It must be used so that the dashboard created at the end of the exercise works correctly.

+   10. Navigate to the Row Detail worksheet and select the Superstore dataset

+   11. Create a crosstab view that displays **Customer Name**, **Order ID**, **Row ID**, **Profit**, and **Sales**. One quick way to create this view is to double-click on each field.

+   12. Navigate to the Agg Dash dashboard and place each of the three worksheets on the dashboard

+   13. Create a blend relationship between **City** and **State** from the **Superstore** and **City Agg** data source: