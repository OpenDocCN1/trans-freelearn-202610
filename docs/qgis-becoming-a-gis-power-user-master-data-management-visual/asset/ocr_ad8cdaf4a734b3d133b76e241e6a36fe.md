# Estimating Unknown Values

# # Sampling the index grid by points

Now that we have regular points, we can attach the grid values to them using the following steps:

+   1. Add all the calculated grids to the map (`calc10` to `calc30`) if they were not already added (navigate to **Layer | Add Layer | Add Vector Layer**).
+   
+   2. Search for **points** under the **Processing Toolbox** pane. Ensure that the **Advanced Interface** is selected from the dropdown at the bottom of the pane.
+   
+   3. Navigate to **SAGA | Shapes - Grid | Add grid values to points**.
+   
+   4. Select the **sample layer** of regular points, which we just created. Following is a screenshot of this, and you will need to execute the following code:

```
Select all grids (calc10-calc30)
```

![](bbox=[0.266, 0.375, 0.735, 0.751])

+   5. Save the output result to `c6/data/output/sample_data.shp`.