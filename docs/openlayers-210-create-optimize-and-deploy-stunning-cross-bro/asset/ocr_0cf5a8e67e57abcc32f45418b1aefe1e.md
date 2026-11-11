# What Just Happened?

We just used the minResolution and maxResolution properties to specify the zoom levels of our map. There is another way, apart from using resolutions, to specify zoom levels and that is by using the scales property.

# scales

```
scales: {Array}
```

To specify the scales, pass in an array of scale values. When using this property, do not use the minResolution, maxResolution, minScale, maxScale, numZoomLevels, minExtent or resolutions properties. If these properties are set, they override the scales property and it will not be used. You should also specify the unit property if your projection is using units other than 'degrees'.

Like the resolutions array, scales are ordered from highest to lowest—most zoomed out to most zoomed in. You can use map.getScale() to get the scale of the current zoom level.

# maxScale

```
maxScale: {Float}
```

This specifies the maximum scale of the map. This property is similar to maxResolution—it limits how far out the user can zoom the map. If you specify the maxScale property, setting the maxResolution or minResolution properties may cause errors. Use either scales or resolutions, but avoid mixing them.

# minScale

```
minScale: {Float}
```

This property specifies the map's minimum scale. More specifically, it determines how far the user can zoom in. If you set this property, the same idea applies as with maxScales—do not mix scales and resolutions properties.

![](bbox=[0.21, 0.7, 0.26, 0.73])

> When setting maxScale and minScale, the minScale value should be larger than the maxScale value.