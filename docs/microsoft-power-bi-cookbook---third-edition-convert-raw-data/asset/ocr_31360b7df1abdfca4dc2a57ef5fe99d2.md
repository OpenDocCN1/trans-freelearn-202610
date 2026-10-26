+   6. Select any visual and click **Edit interactions** in the ribbon of the **Format** tab to toggle off editing interactions.
+   7. Save your work.

# How it works

In this example, it was determined that the KPI visualizations should always show the last full month's statistics for all products and all countries, states/provinces, and cities. To ensure this is the case, interactivity with all other visualizations on the pages is set to **None** instead of the default **Filter**, using the **Edit interactions** feature on Power BI Desktop.

It is important to realize that DAX measures can override cross-filtering behavior by using filter functions such as ALL, ALL EXCEPT, and REMOVEFILTERS. In fact, there actually was no need to set the interaction between the **Calendar Month** slicer and the KPI visuals to **None** because the DAX measures that drive the KPI visuals include the use of the ALL function for the Dates table as part of their calculations.

# There’s more…

The cross-filtering interactions between visualizations have the options of **Filter**, **Highlight**, and **None**—some visualizations, such as KPI visualizations, only include Filter and None options, whereas other visualizations, such as bar and column charts, include a **Highlight** option as well. To see the **Highlight** option, do the following:

+   1. On the Europe page, use the **Visualizations** pane to create a clustered column chart using the Year Month column from the Dates table as the X-Axis and the Internet Net Sales and Internet Plan measures from the Internet Sales table as the Y-Axis.
+   2. In the **Format** pane, on the **Visual** sub-pane, expand the **Legend** format card and change Position to Bottom left, turn off Title on the X-axis, turn off Title and Values for the Y-axis, expand Data labels and then Values, and change Display units to Thousands, Value decimal places to 0, and Orientation to Horizontal.
+   3. On the General sub-pane, expand the Title format card and change Text to Internet Sales to Plan with Horizontal alignment set to Center.
+   4. Expand the Properties format card and set Height to 241 and Width to 831. Under Position, set Horizontal to 449 and Vertical to 47.
+   5. Create a stacked bar chart with the Sales Territory Country column from the Sales Territories table as the Y-Axis, and the Internet Net Sales measure from the Internet Sales table and the Reseller Net Sales measure from the Reseller Sales table as the X-Axis.
+   6. Refer to steps 2, 3, and 4 to set the legend position to Bottom left, turn off Title on the Y-axis, turn off Title and Values on the X-axis; expand Data labels and change Display units to Thousands; change Title Text to Internet and Reseller Sales by Country with a Horizontal alignment of Center. Then, set Height to 212, Width to 527, Horizontal to 753, and Vertical to 295.
+   7. Select the Stacked bar chart visual and click Edit interactions in the ribbon of the Format tab.