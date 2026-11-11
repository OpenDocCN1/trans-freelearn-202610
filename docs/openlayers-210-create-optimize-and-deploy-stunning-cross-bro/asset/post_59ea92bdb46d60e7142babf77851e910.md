
The possible values are, basically, anything that you find in the API documentation for either the base **Layer** class (at http://dev.openlayers.org/docs/files/OpenLayers/Layer-js.html), or the specific subclass you're working with, e.g., the **WMS Layer Class**, at http://dev.openlayers.org/docs/files/OpenLayers/Layer/WMS-js.html.

![](img/1b8cd151c9f278555f9cf255b20d244e_75_0.png)

> You can use this **options** parameter to initialize any of the layer parameters described in the API—the best way to get more familiar with it is to just play around with the different properties listed in the docs. The Layer class docs can be found at http://dev.openlayers.org/docs/files/OpenLayers/Layer-js.html.

## Parameters versus arguments

These two terms are often confused and used interchangeably (but usually, that's ok). **Parameters** are what the items are called during the class or function definition, but when we call the function, the actual values of the parameters are referred to as **arguments**. For example, this is how we create a function in JavaScript:

```
function add_numbers(a, b){
  return a + b;
}
```

This is referred to as the `add_numbers` function definition. Here a and b are referred to as **parameters**. Now, take a look at how we call it:

```
var the_sum = add_numbers(13, 37);
```

Here, 13 and 37 are referred to as the **arguments**. In the `add_numbers` function, 13 acts as a and 37 acts as b.

Both terms are technically talking about the same thing. **Parameter** is the term to use when talking about function definitions, and **argument** is the term to use when talking about function calls. The distinction is sort of like that of a meteoroid (when it's in space), a meteor (when it's in the atmosphere), and a meteorite (when it hits the ground)—these three terms refer to the same object.

Now, let's take a look at some of the different arguments we can pass in when creating our layer objects.

## Time for Action – configuring the options parameter

The options parameter is something that is present in all Layer classes, so let's get a bit more familiar with it. I suggest opening up the Layer documentation and following along, and even trying to add in the options yourself from the possible list of layer properties (at http://dev.openlayers.org/docs/files/OpenLayers/Layer-js.html).

- 1. Use the template (or code from Chapter 1 with the WMS layer code removed). We'll be adding some WMS layers, and we'll refer to this file as `chapter_3_ex2_options_config.html`.
- 2. First we'll add in a layer that contains the 'basic' layer from the WMS server, like in the previous example.
- 3. Next we'll create another layer object. We'll create a layer using labels, like in the previous example. This time though, let's set the layer's options to include `visibility: false`. This will cause the layer to be hidden by default. The layer definition should now look like:
- 4. Time to create another layer. We'll set the layer params to ask the WMS service for the stateboundary layer. Then, we'll specify the options so that it won't display in the layer switcher control (via `displayInLayerSwitcher: false`) and set a minimum scale at which it will be visible (via `minScale`). That means this layer will only show up once we've reached a certain scale. Add the following to your `init()` function.

```
// Setup our two layer objects
var wms_layer_map = new OpenLayers.Layer.WMS(
  'Base layer',
  'http://vmap0.tiles.osgeo.org/wms/vmap0',
  {layers: 'basic'},
  {isBaseLayer: true}
);
```

```
var wms_layer_labels = new OpenLayers.Layer.WMS(
  'Location Labels',
  'http://vmap0.tiles.osgeo.org/wms/vmap0',
  {layers: 'clabel,ctylabel,statelabel',
  transparent: true},
  {visibility: false, opacity:0.5}
);
```

```
var wms_state_lines = new OpenLayers.Layer.WMS(
  'State Line Layer',
  'http://labs.metacarta.com/wms/vmap0',
  {layers: 'stateboundary',
transparent: true},
{displayInLayerSwitcher: false,
minScale: 13841995.078125}
);
```

- 5. Now let's add a layer that will show a different layer from the WMS service and we'll set the layer object's opacity to .8 (or 80 percent):

```
var wms_water_depth = new OpenLayers.Layer.WMS(
'Water Depth',
'http://labs.metacarta.com/wms/vmap0',
{layers: 'depthcontour',
transparent: true},
{opacity:0.8}
);
```

- 6. Finally, we'll create a layer object that shows some road layers from the WMS service and has an options object containing the transitionEffect: resize property. This causes the layer to have a 'resize' animation when zooming in or out.

```
var wms_roads = new OpenLayers.Layer.WMS(
'Roads',
'http://labs.metacarta.com/wms/vmap0',
{layers: 'priroad,secroad,rail',
transparent: true},
{transitionEffect:'resize'}
);
```

- 7. Now we just add the layers to the map (replace the previous addLayers function call with this one):

```
map.addLayers([
    wms_layer_map,
    wms_layer_labels,
    wms_state_lines,
    wms_water_depth,
    wms_roads]);
```

- 8. Save the file and then open it in your web browser.

- 9. Zoom in a few times around the Gulf of Mexico. You'll start to notice black contour lines in the ocean when you zoom in, and you should see a resize effect from the base ground layer. Depending on where you zoom, you should see something like this:

![](img/1b8cd151c9f278555f9cf255b20d244e_78_0.png)

## What Just Happened?

We just created another map with a few more layers and demonstrated some more layer options. The map looks slightly different than the first example's map because we are using different WMS server side layers (which are configured in each layer's params argument).

For now, let's focus on the fourth argument passed in—the options argument. This argument is used in all Layer classes, so when we move on to other types of Layers you'll be able to use all the same properties we're talking about. The options argument controls the layer's object's properties.

## Configuring layer options

Let's take a look at a couple of the layer's options argument we passed in during the previous example.

![](img/1b8cd151c9f278555f9cf255b20d244e_79_0.png)

> Pay close attention to the commas. Both arguments and key:value pairs are separated by a comma, but make sure you do not have a comma after the last pair. Trailing commas will usually break your map and are usually a pain to debug—so if your map isn't working and you don't see any helpful error messages in Firebug, check for trailing commas. A great site to help with your code is http://jslint.com.

## wms_state_lines layer options

This layer's options are:

```
{ displayInLayerSwitcher: false, minScale: 13841995.078125 }
```

The first property, displayInLayerSwitcher, can be either true or false, and determines if the layer will appear in the layer switcher control (if there is a layer switcher control). In our example, we do have a layer switcher control and you'll notice that we don't see it in the list of layers. The layer is still there, and we can programmatically turn it on or off through the setVisibility() function, e.g. wms_state_lines.setVisibility(false). In fact, when you turn on or off a layer through the layer switcher control, this setVisibility function is what is being called. This property is useful when you want to include layers in your map that you don't want to let the user control.

The second property is minScale: 13841995.078125. The value for this property is float, a number that can contain a decimal point. This minScale property determines the minimum scale the map must be at before the layer is displayed, which basically means how far we must be zoomed in before the layer will be turned on. There is also a maxScale property which determines how far we can zoom in before the layer is turned off. The term for this behavior is referred to as scale dependency.

## Scale dependency

Scale dependency can be controlled either from the client or server side (or both). You'll notice that the wms_water_depth layer (with black contour lines in the water to specify depth) does not turn on until we start to zoom in. We haven't set the minScale property for the wms_water_depth layer, so why don't we see it at all zoom levels? The reason is WMS server has its own scale dependencies on the server side, so even if we wanted to see this layer when the map is zoomed out we can't because the server does not allow it.
