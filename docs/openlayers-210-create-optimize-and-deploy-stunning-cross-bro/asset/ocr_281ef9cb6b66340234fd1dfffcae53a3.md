# Have a Go Hero – style layers

Now that you're a little more familiar with how to use symbolizers, open up the previous example and change some of the symbolizers. Test different property and value combinations—different colors, opacities, background colors, etc. See if you can come up with something that looks a little better than grey and black circles.

Before we go into attribute replacement, our next topic, let's look at the remaining symbolizer properties we haven't covered yet.

# Remaining symbolizer properties

+   - **backgroundGraphic**: Specifies the URL of an image to use. This will be placed under the `externalGraphic` (if one is set).
+   - **backgroundGraphicZIndex**: Specifies the z-index (order of what gets placed on each other) of the `backgroundGraphic`.
+   - **backgroundHeight**: Determines the height of the background image. If none is specified, `graphicHeight` is used.
+   - **backgroundWidth**: Height of the background image, if one exists. If this property is not set and an image is provided, the `graphicHeight` property will be tried to use.
+   - **backgroundXOffset**: Specifies how far, in the X direction, to offset the image (in pixels).
+   - **backgroundYOffset**: Specifies how far, in the Y direction, to offset the image (in pixels).
+   - **display**: If set to `none`, the symbolizer styles will not be applied.
+   - **externalGraphic**: Specifies the URL of an image to use (instead of the stroke and fill properties) when feature points are shown.
+   - **fill**: If set to `false`, no fill will be shown.
+   - **graphic**: Setting to `false` will disable any graphics from being shown.
+   - **graphicHeight**: The height, in pixels, of an external graphic (if it exists).
+   - **graphicOpacity**: Specifies the opacity of associated graphic, ranging from 0 to 1 (1 being fully opaque, 0 being fully transparent).
+   - **graphicTitle**: This specifies tooltip text that will appear when mousing over an external graphic (if one is provided).
+   - **graphicWidth**: The width, in pixels, of an external graphic (if it exists).
+   - **graphicXOffset**: How far, in the X direction, to offset the external graphic (in pixels).
+   - **graphicYOffset**: How far, in the Y direction, to offset the external graphic (in pixels).