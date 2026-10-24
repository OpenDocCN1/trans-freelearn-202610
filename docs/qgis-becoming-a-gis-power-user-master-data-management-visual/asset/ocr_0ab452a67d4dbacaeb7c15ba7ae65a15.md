+   4. Create a `rank` field, editing each value manually according to the `_mean` field created by the zonal statistics step. This is a measure of the mean suitability per cell. We will use this field for a label to communicate the relative suitability to a general audience; so, we want a rank instead of the rough mean value.

+   5. Now, label the layer.

+   1. Under **Layer Properties**, activate the **Labels** tab.
+   2. Choose the `rank` field as the field to label.
+   3. Add any other formatting, such as label placement and buffer (halo) using the inner tabs within the label tab dialog, as shown in the following screenshot:

![](bbox=[0.17, 0.35, 0.83, 0.7])