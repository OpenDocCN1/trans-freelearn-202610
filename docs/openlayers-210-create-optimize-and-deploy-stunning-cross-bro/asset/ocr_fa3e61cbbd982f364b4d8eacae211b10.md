| Name | Type | Description | Default Value |
|---|---|---|---|
| displayProjection | `{OpenLayers. Projection}` | Controls what projection the coordinates will be displayed in. Any projection other than `EPSG:4326` or `EPSG:900913` will require the Proj4js library to properly re-project the coordinates. If this property is not set, the map's projection will be used. |  |
| div | `{HTML Element}` | If specified, the HTML Element to place this control in. |  |

# OpenLayers.Control.Navigation

The **Navigation** control is one of the four controls added by default to your map. This control handles map navigation via mouse input, such as dragging the map and scrolling to zoom in. Let's take a look at a couple of the properties.

# Navigation properties

| Name | Type | Description | Default Value |
|---|---|---|---|
| documentDrag | `{Boolean}` | Determines whether the map should be allowed to be dragged when the mouse exits the map viewport (i.e., the map HTML div element). | `false` |
| handleRightClicks | `{Boolean}` | If set to turn, the map will intercept right clicks. On a double right click, the map will zoom out. | `false` |
| zoomWheel | `{Boolean}` | Specifies if the mouse scroll wheel will make the map zoom in. | `true` |
| autoActivate | `{Boolean}` | Determines whether the control should be activated when it is added to the map. It can be useful when adding this control to a panel. | `true` |