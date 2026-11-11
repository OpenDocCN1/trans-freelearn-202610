# LayerSwitcher functions

There are a couple of functions we can manually call to show or hide the LayerSwitcher control.

+   - **minimizeControl()**: When called, it will minimize the layer switcher control. Especially useful if you want to programmatically hide the layer switcher control.
+   - **maximizeControl()**: Shows the layer switcher control; it essentially does the opposite of `minimizeControl()`.

# OpenLayers.Control.MousePosition

This control displays the longitude and latitude of where the mouse is currently located. By default, the control displays the position on the bottom right hand side of the map.

# MousePosition properties

| Name | Type | Description | Default Value |
|---|---|---|---|
| prefix | `{String}` | The string that will be added to the beginning of the coordinate text. | `''` |
| suffix | `{String}` | The string that will be added after the coordinate text. | `''` |
| emptyString | `{String}` | Text that will be displayed instead of the coordinates if the mouse is moved outside of the map. By default, when the mouse is moved outside of the map the last known coordinates will remain displayed. | null |
| separator | `{String}` | The string that will be placed in between the longitude and latitude. | `','` |
| numDigits | `{Integer}` | Specifies the numbers of digits after the decimal point (i.e., precision), of the coordinate displayed. | 5 |