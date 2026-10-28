![](bbox=[0.175, 0.08, 0.825, 0.54])

Figure 9.3: Relationships between Customer, Product, and Internet Sales

In the preceding data model, we have the following relationships:

+   - A one-to-many relationship between Customer and Internet Sales
+   - A one-to-many relationship between Product and Internet Sales
+   - A many-to-many relationship between Customer and Product (via the Internet Sales table)

The first two relationships are trivial, as we can visually see them in the data model. However, the latter is somewhat of a conceptual relationship handled by the Internet Sales table. From a Star Schema standpoint, we do not call the Internet Sales table a bridge table, but the principles remain the same. In data modeling using the Star Schema approach, a bridge table is