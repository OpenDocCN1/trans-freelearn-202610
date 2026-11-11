| Name | Type | Description | Default Value |
|---|---|---|---|
| mapOptions | `{Object}` | This is an object just like the options property of the map object. It is an anonymous object that can contain the same options the map can contain. Setting some of these options can be very useful, especially the `numZoomLevels` property. For example, `mapOptions: {numZoomLevels: 1}` will cause the overview map never to zoom in or out, staying at the max extent. | `{}` |
| autoPan | `{Boolean}` | This property determines whether to automatically pan the overview map to keep the extent rectangle in the middle. Setting this value to `true` will mimic the behavior of the overview map in Google Maps. | `false` |
| div | `{HTML Element}` | If specified, the HTML Element to place this control in. If not specified, the **OverviewMap** control goes in the bottom right hand of the map. |  |

# OverviewMap functions

+   - **minimizeControl()**: When called, it will minimize the layer switcher control. It is especially useful if you want to programmatically hide the layer switcher control.
+   - **maximizeControl()**: It shows the layer switcher control, it essentially does the opposite of `minimizeControl()`.

# OpenLayers.Control.PanPanel

This control adds a pan panel to the top left hand side of the map. It contains four images: up, left, right, and down arrows. Clicking on an arrow will pan the map in the arrow's direction.