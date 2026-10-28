![](bbox=[0.177, 0.083, 0.822, 0.428])

# Figure 2.10: Product and Internet Sales

The next step is to create a measure to calculate the average product standard cost:

```
Avg. Product Standard Costs =
AVERAGE('Internet Sales'[ProductStandardCost])
```

Let’s test the created measure with the following steps:

+   1. Put a matrix visual on the report canvas.
+   2. Put the Product Category, Product Subcategory, and Product columns from the Product table into the Rows section of the matrix visual.
+   3. Put the Avg. Product Standard Costs measure in the Values section of the matrix.

The following screenshot illustrates the steps: