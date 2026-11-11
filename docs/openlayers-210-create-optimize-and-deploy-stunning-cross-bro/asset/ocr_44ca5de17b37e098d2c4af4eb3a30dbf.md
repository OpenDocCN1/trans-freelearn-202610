# Time for Action – changing the Renderers array

Let's use the Canvas renderer and see what our map looks like.

+   1. Make a copy of the previous example. We'll just be changing one line of the code.
+   2. The only thing we'll need to do is set the renderers property when instantiating the vector layer. By default, it is set as ['SVG', 'VML', 'Canvas']. Let's make Canvas the default renderer. Replace your vector layer instantiation code with:
+   3. Open up the map and draw some points. It may look similar, but you will probably notice that the lines don't look as sharp as they do when using the SVG renderer. If you use Firebug to inspect the map element, you'll see a <canvas> tag:

```
var vector_layer = new OpenLayers.Layer.Vector('Basic Vector Layer', {
  renderers: ['Canvas', 'SVG', 'VML']
});
```

![](bbox=[0.191, 0.424, 0.811, 0.645])

# What Just Happened?

You just saw how easy it is to change the renderer used by the vector layer—although it is not usually necessary to change it.

Now that we know how the vector layer is rendered, let's talk about the class itself.