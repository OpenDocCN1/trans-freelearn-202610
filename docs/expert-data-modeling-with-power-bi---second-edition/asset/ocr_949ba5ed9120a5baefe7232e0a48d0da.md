![](bbox=[0.18, 0.08, 0.82, 0.23])

Figure 9.9: Analyzing customer purchases and sales reasons for a specific product

As you can see, the preceding analysis was made possible only by correctly handling the many-to-many relationship between the Internet Sales and Sales Reasons tables via the Internet Sales Reasons bridge table.

# Hiding the bridge table

After implementing the many-to-many relationship in the data model, it is a good idea to hide the bridge table from the data model. We only have the bridge table in our data model as it carries the key columns of both tables participating in the many-to-many relationship. Hiding the bridge table also avoids confusion for other report creators who connect to our dataset to build the reports. To hide a table, we only need to switch to the Model view and click the hide/unhide (👁️) button at the top right of the table. The following diagram shows the data model after hiding the Internet Sales Reasons table: