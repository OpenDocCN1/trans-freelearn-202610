# OpenLayers.Control.KeyboardDefaults

This control adds panning and zooming functionality, controlled by the keyboard. By pressing the arrow keys, plus/minus keys, or home keys, the map will be moved or zoomed. There is one relevant property we should take a look at.

# KeyboardDefaults properties

| Name | Type | Description | Default Value |
|---|---|---|---|
| slideFactor | {Integer} | Sets the amount of pixels to slide the map by. | 75 |

# OpenLayers.Control.LayerSwitcher

This control will create a layer list that allows you to turn on/off map layers. By default, it creates a toggle-able window in the map, but you can pass in a div HTML element and the layer list will be created in it.

# LayerSwitcher properties

| Name | Type | Description | Default Value |
|---|---|---|---|
| ascending | {Boolean} | Changes the order the layers are displayed. | true |
| roundedCorner | {Boolean} | Specifies if rounded corners should be created for the layer switcher window. | true |
| div | {HTML Element} | If specified, the HTML Element to place this control in. If you use this, you should set the roundedCorner property to false, otherwise, rounded corners will be created inside the target div. Example: document.getElementById('map_control_div'); | null |