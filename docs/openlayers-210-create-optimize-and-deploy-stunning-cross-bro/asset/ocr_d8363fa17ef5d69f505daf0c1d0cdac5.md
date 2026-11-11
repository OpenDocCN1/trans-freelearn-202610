+   - **strokeColor**: Color of the stroke, a six digit RGB value. Example: `#000000` would be black (`00` red, `00` green, and `00` blue.) `#ffffff` would be white.
+   - **strokeDashstyle**: Type of style applied to the strokes. Can be either `'dot'`, `'dash'`, `'dashdot'`, `'longdash'`, `'longdashdot'`, or `'solid'`.
+   - **strokeLinecap**: The type style to be applied to the line caps. Can be either `'but'`, `'round'`, `'square'`.
+   - **strokeOpacity**: Opacity of strokes, ranging from 0 to 1 (1 being fully opaque, 0 being fully transparent).
+   - **strokeWidth**: Stroke width, in pixels.

# Time for Action – common style examples

Let's take a look at what some of these common properties do through an example. We'll also demonstrate how to use two different intents—the `default` intent and the `selected` intent.

1. Make a copy of the previous example's file. We'll be using the same code, but modifying the feature styles a bit. We'll also use the `select` intent, so we'll need two style objects. Let's first recreate the default style object:

```
var vector_style = new OpenLayers.Style({
  'cursor': 'pointer',
  'fillColor': '#787878',
  'fillOpacity': .8,
  'fontColor': '#343434',
  'pointRadius': 14,
  'strokeColor': '#232323',
  'strokeDashstyle': 'dot',
  'strokeWidth': 3
});
```

2. Now, let's create a style object to use when the feature is selected. We'll change the border to solid, change the fill color, increase the point's radius a little bit, change the `graphicName` to `'square'`, and add a label that will display 'X'.

```
var vector_style_select = new OpenLayers.Style({
  'fillColor': '#ffffff',
  'fillOpacity': .9,
  'graphicName': 'square',
  'label': 'X',
  'pointRadius': 16,
  'strokeColor': '#343434',
  'strokeDashstyle': 'solid',
```