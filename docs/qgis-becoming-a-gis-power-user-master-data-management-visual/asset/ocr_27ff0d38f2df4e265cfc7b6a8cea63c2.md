Algorithms in the Processing framework systematically ignore no-data cells, and do not use their values. You can clearly see this in the preceding example. A large part of the cells in the layer have a value of 1 (the ones that belong to the watershed), but many of them have a value of -99999. The average value of the cells should then be different from 1, but as -99999 is defined as the no-data value, all cells with this value are ignored. The average of the layer is, therefore, equal to 1.

# There's more...

Null values should be considered not only when performing an analysis, but also when we just want to render a layer that contains them.

# Controlling the rendering of null values

Null values are also considered separately when rendering a raster layer. You can choose to select them using a given color (as set by the current color palette), or to not render them at all. To make all cells with null values transparent, open the layer properties and go to the Transparency section. Make sure that the No data value checkbox is checked, as shown in the following screenshot:

![](bbox=[0.175, 0.445, 0.825, 0.807])