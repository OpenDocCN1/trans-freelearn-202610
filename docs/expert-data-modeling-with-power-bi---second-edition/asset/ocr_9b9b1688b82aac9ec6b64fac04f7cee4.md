We can also see the DAX expressions that Power BI has automatically generated to create the new calculated table, as illustrated in the following image:

![](bbox=[0.21, 0.17, 0.8, 0.28])

Figure 2.34: The changes defining Fields Parameters make

So far, we have created the required Fields Parameters. Let us finish the implementation of the scenario by following these steps:

+   8. Put an area chart on the report page
+   9. Put the Full Date column from the Date table to the x axis
+   10. Put the Internet Sales Variance PoP column from the Internet Sales Variance PoP table to the y axis

As the following image illustrates, the chart shows both Internet Sales MoM Variance and Internet Sales YoY Variance measures. So the user can select either measure to show on the chart, select both, or select none. But the scenario requires the user always to select only one measure. The following steps show how we achieve this:

+   11. Select the Slicer
+   12. Click the Format visual tab from the Visualizations pane
+   13. Expand the Slicer settings
+   14. Toggle on the Single select option from the Selection section

Now, the user can select the desired measure to show on the area chart. The following image shows the preceding steps: