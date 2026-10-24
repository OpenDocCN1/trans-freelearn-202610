+   4. Provide a specific field expression: `cost < [break value]` (for example, `cost < 15`).
+   5. Click on OK to select the objects in the layer that matches the expression.
+   6. Navigate to **Processing Toolbox | Concave hull**.
+   7. Input the following parameters for **Concave hull**. All other parameters can be left at their defaults:
+      - **Input point layer: isochron_centroids**
+      - Select **Split multipart geometry into singleparts geometries**
+      - **Concave hull** (the output file) could be similar to `c4/data/output/isochrone45.shp`
+      - Click on Run, as shown in the following screenshot:

![](bbox=[0.184, 0.373, 0.816, 0.739])