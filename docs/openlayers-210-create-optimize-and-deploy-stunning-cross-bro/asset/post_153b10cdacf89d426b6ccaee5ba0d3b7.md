
## What is a 'Vector'?

In terms of graphics, there are essentially two types of images: raster and vector. Most images you see are raster images—meaning, basically, they are comprised of a grid of pixels and their quality degrades as you zoom in on them. A photograph, for example, would be a raster image. If you enlarge it, it tends to get blurry or stretched out. The majority of image files— .jpegs, .png, .gifs, any bitmap image—are raster images.

A vector, on the other hand, uses geometrical shapes based on math equations to form an image. Meaning that when you zoom in, the quality is preserved. If you were to zoom in on a vector image of a circle, the lines would always appear curved—with raster image, the lines would appear straight, as raster images are made up of a grid of colors. Vector graphics are not constrained to a grid, so they preserve shape at all scales.

## Time for Action – creating a Vector Layer

Let's begin by creating a basic vector layer. In this example, after you add some points and other feature types to your vector layer, try to zoom in. You'll notice that the points you added don't lose quality as you zoom in. We'll go over how it works afterwards.

- 1. We'll start off by using a basic WMS layer:
```
var wms_layer = new OpenLayers.Layer.WMS(
    'OpenLayers WMS',
    'http://vmap0.tiles.osgeo.org/wms/vmap0',
    {layers: 'basic'},
    {}
);
```
- 2. Now, let's create the vector layer itself. We'll use the default projection and default values for the vector layer, so to create the layer all we need to do is create it:
```
var vector_layer = new OpenLayers.Layer.Vector('Basic Vector Layer')
```
- 3. Add the layers to the map now:
```
map.addLayers([wms_layer, vector_layer]);
```
- 4. If we looked at the map now, we would just see a simple map—our vector layer does not have any data loaded into it, nor do we have any controls to let us add vector data.

- 5. Let's add the **EditingToolbar** control to the map, which allows us to add points and draw polygons on a vector layer. To do so, we just need to instantiate an object from `OpenLayers.Control.EditingToolbar` and pass in a vector layer. We'll pass in the `vector_layer` object we previously created:
```
map.addControl(new OpenLayers.Control.EditingToolbar(vector_layer));
```
- 6. Take a look at the map now. You should see the `EditingToolbar` control (which is basically a panel control with control buttons). Selecting different controls will allow you to place vector objects (called features) on the vector layer. Play around with the `EditingToolbar` control and place a few different points / polygons on the map:

![](img/1b8cd151c9f278555f9cf255b20d244e_249_0.png)

- 7. Now, one more step. You've placed some features (points / polygons / lines / etc.) on the map, but if you were to refresh the page they would disappear. We can, however, get the information about those features and then export it to a geospatial file. We'll work with files later, but for now let's grab the information about the features we've created. To access the information about the vector layer's features, all we need to do is access its `features` array. In Firebug, type and run the following:
```
map.layers[1].features
```

- 8. You should see a bunch of objects listed, each object is a feature you placed on the map:
```
[Object { layer=Object, more...}, Object { layer=Object, more...}, Object { layer=Object, more...}, ...]
```
- 9. Now, if you expand one of those objects, you'll get the information about a feature. The `geometry` property is an anonymous object each feature has which contains geometry information. You can also see the methods of the feature objects—try playing around with different functions. You can access the individual features by using `map.layers[1].features[x]`, where x is the index of the feature in the features array. For instance, to destroy the first feature which we added to the map we could use:
```
map.layers[1].features[0].destroy();
```

## What Just Happened?

We just demonstrated how to create a basic vector layer and added features to it using the `EditingToolbar` control. Using the `features` array of the vector layer, we also destroyed some features. As you've just seen, it's not terribly difficult to start using the vector layer—pretty easy, in fact. The rest of this chapter will more or less build on what we just saw, so there's no need to be intimidated.

## Pop Quiz – why use a Vector Layer?

Vector layers tend to be very fast, as the data can be stored entirely on the client. Interaction happens instantly, which can greatly enhance the user's experience. Come up with a couple of cases where you would use a vector layer.

Now, before we jump into more advanced uses of the vector layer, let's see how it actually works first.

## How the Vector Layer works

There are primarily four things we need to cover to understand how the vector layer works.

- How the Vector Layer is rendered
- The Vector Layer class itself
- The Geometry and Feature classes
- How to use the related Strategy, Protocol, and Format classes

Let's go over rendering—fortunately, there's not much to it.

## How the Vector Layer is rendered

As we discussed earlier, the vector layer doesn't use raster graphics. Other layer types use the <img> tag (image tag) to show images—in HTML, the image tag will only display raster images. So, we can't just use <img> tags like other layers. Instead, we have to use a vector image renderer. As we mentioned before, the vector data is not just an image, and can contain additional information such as the coordinates of the data.

The vector data must be rendered to be seen. OpenLayers supports three ways to render the vector layer: SVG, Canvas, and VML.

## SVG

The default way to render the vector layer is to use the SVG renderer, which makes use of the <svg> tag. SVG is an acronym for Scalable Vector Graphics, and allows us to render vector images in the browser. All browsers (except Internet Explorer) support SVG, so by default OpenLayers uses SVG to render the vector layer.

## Canvas

We can use the Canvas renderer, which makes use of the <canvas> HTML tag. Using this renderer tends to be a little slower however, as the vector is actually turned into a raster with the canvas tag—but this may be desirable in some cases.

## VML

Internet Explorer does not follow web standards, and at the time of writing did not support SVG or Canvas. Fortunately, by default, OpenLayers will detect if the user's web browser does not support modern technologies and a fall back renderer called VML will be used (which is similar to SVG, but is Microsoft specific). SVG tends to be faster than VML, so the only case where you would want to use VML over SVG is when Internet Explorer is used.

## 'Renderers' array

When you create a vector layer, it looks for an array called renderers which contains the names of renderers, in order, to use. By default, the renderers array is set as:
```
["SVG", "VML", "Canvas"]
```
This means that OpenLayers will try to use SVG first, and if the browser does not support SVG it will fall back on VML—and if it does not support VML, it will try to use Canvas.

## Time for Action – changing the Renderers array

Let's use the Canvas renderer and see what our map looks like.

- 1. Make a copy of the previous example. We'll just be changing one line of the code.
- 2. The only thing we'll need to do is set the renderers property when instantiating the vector layer. By default, it is set as ['SVG', 'VML', 'Canvas']. Let's make Canvas the default renderer. Replace your vector layer instantiation code with:
- 3. Open up the map and draw some points. It may look similar, but you will probably notice that the lines don't look as sharp as they do when using the SVG renderer. If you use Firebug to inspect the map element, you'll see a <canvas> tag:
```
var vector_layer = new OpenLayers.Layer.Vector('Basic Vector Layer', {
  renderers: ['Canvas', 'SVG', 'VML']
});
```
![](img/1b8cd151c9f278555f9cf255b20d244e_252_0.png)

## What Just Happened?

You just saw how easy it is to change the renderer used by the vector layer—although it is not usually necessary to change it.

Now that we know how the vector layer is rendered, let's talk about the class itself.
