You only need two measures on **Rows** and **Columns** as well as a dimension to partition the points in your view. Then, you go to **Analysis** and drag the **Trend Line** onto your screen and select **Linear**:

![](bbox=[0.16, 0.115, 0.89, 0.655])

# Figure 14.13: Scatterplot

Notice that this screenshot is from Tableau. You can access it by clicking on the **Scatterplot** worksheet in the workbook associated with this chapter. It's a simple scatterplot with trend lines turned on. Trend lines, in Tableau, default to linear but also include logarithmic, exponential, and polynomial options, which are all examples of regression analysis. By accessing **Worksheet | Export | Data** in a visualization utilizing a trend line, you can generate an **Access** database with predictions and residuals for marks on the view. But this is a tedious process and does not give a robust, dynamic solution for implementing more vigorous uses of linear regression. Using R provides much more flexibility.