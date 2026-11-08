# Extended XY line chart

The extended XY line chart allows the rendering of three additional chart types: step area charts, step charts, and difference charts. The step chart types display an XY series dataset as a set of steps, and the difference chart renders two XY series and highlights the differences between the two. These chart types utilize the common properties defined earlier, including the XY series dataset properties as default. As alternative datasets, you can also decide to use the XYZ series dataset or the times series dataset. You can change the dataset using the drop-down menu in the upper part of the Primary DataSource tab.

Concerning the available properties for the extended XY line chart, in the following description are listed all the differences from the common list introduced in the beginning of the chapter:

+   - The title set has the same properties as described for the bubble chart (the title common set and `pos-title` in addition)
+   - The options set has the same properties as described for the line chart, including `ext-chart-type` (with possible values `StepChart`, `StepAreaChart`, and `DifferenceChart`) and `stacked` (introduced in the options group for the bar chart)
+   - The general set has the same properties as described for the bar chart, excluding `gridlines`
+   - The X axis set has the same properties as described for the XY bar chart
+   - The Y axis set has the same properties as described for the bar chart, excluding `y-scale-factor`
+   - The values set is a subset of the properties described for the bar chart, limited to `show-item-labels`, `item-label-font`, `url-formula`, and `tooltip-formula`