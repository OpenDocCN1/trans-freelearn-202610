# Graticule properties

| Name | Type | Description | Default Value |
|---|---|---|---|
| intervals | {Float
{Array}} | An array of floats of possible graticule widths, in degrees. The different widths will be used depending on the current map zoom levels. Example: intervals: [50,30,15,7,4,2,1,.5] | [45,30,20,10,5,2,1,0.5,0.2,0.1,0.05,0.01,0.005,0.002,0.001] |
| displayInLayerSwitcher | {Boolean} | If set to true, the graticule control will be displayed in the layer switcher. | true |
| visible | {Boolean} | Determines if the graticule control should be visible or not. | true |
| numPoints | {Integer} | Specifies the number of points to use in each of the graticule lines. | 50 |
| targetSize | {Integer} | Determines the maximum size of the grid in pixels on the map. | 200 |
| layerName | {String} | Specifies the layer name that will be displayed in the LayerSwitcher control. | null |
| labeled | {Boolean} | Determines if the graticule lines should be labeled. | true |
| labelFormat | {String} | The format of the coordinates the labels will display in. Possible values, for example, would be 'd', 'dm', or 'dms', which represent 'degree', 'minutes', and 'seconds', which determine how precise the coordinates will be displayed in. | 'dm' |
| lineSymbolizer | {Symbolizer} | Specifies the symbolizer to be used to render the lines. A symbolizer specifies style information. In Chapter 10, we will discuss symbolizers and how to use them in detail. |  |
| labelSymbolizer | {Symbolizer} | The symbolizer used to render labels, which controls the label style. Chapter 10 discusses symbolizers (objects that define style) in detail. |  |