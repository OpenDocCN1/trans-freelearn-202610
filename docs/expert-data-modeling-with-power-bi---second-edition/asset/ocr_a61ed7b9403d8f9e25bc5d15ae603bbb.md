Figure 3.32: Identifying high-cardinality columns in the Customer table

The **CustomerKey** column is not a candidate for removal as it participates in the relationship between the **Internet Sales** and **Customer** tables in the data model. We can remove the **CustomerAlternateKey** column. This is an excessive column with very high cardinality (100% unique values), and it also does not add any value from a data analysis point of view. The two other columns are excellent candidates to discuss with the business to see if we can remove them from the **Customer** table.

If we remove all three columns, we can reduce the file size from 2,485 **kilobytes (KB)** to 1,975 KB. This is a significant saving in storage, especially in larger data models.

# Column profile

So far, we have looked at the **Column quality** and **Column distribution** features. We can also enable the **Column profile** feature to see more information about a selected column’s values. To enable this feature, tick the **Column profile** box under the **View** tab, as illustrated in the following image:

![](bbox=[0.17, 0.57, 0.83, 0.65])

Figure 3.33: Enabling Column profile from Power Query Editor

As the preceding image shows, we can see **Column statistics** and **Value Distribution** by enabling the **Column profile** feature. We can hover over the values to see the count number of that value and its percentage in a flyout menu. We can also take some actions on the selected value by clicking the ellipsis button at the flyout menu’s bottom right, as illustrated in the following image: