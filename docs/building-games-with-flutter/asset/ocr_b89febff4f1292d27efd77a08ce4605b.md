Layers are a feature of Flame that allow us to group things we want to draw together or draw a prerendered graphic that doesn't change much. In your game, you may have a background that you draw once from a combination of sprites or images, but then it is used as a static image that you use as a background and draw the other moving sprites on top.

It would be inefficient to keep creating this background if it isn't changing. So, you can create it once and store it as a layer, which you can draw before you render the other game graphics.

In Flame, there are two types of layers:

+   - PreRenderedLayer – For static images
+   - DynamicLayer – For things that are moving

`PreRenderedLayer` would be suitable for backgrounds due to its static nature.

You may also want to change something in the layer and regenerate the layer, and then cache the resulting image in the layer. For example, you may want to create a weather effect in the game where the raindrops are updated and redrawn on the layer, and then this layer is drawn on top of your game world to give the impression it is raining or snowing. For this type of effect, `DynamicLayer` would be more suitable.

Flame also provides something called layer processors, which allow us to add effects to the entire layer. Currently, the only supported layer processor is called `ShadowProcessor`, which applies a shadow to the entire layer. It is possible to make your own processors though by extending the `LayerProcessor` class if you want to create other processors.

We can use `ShadowProcessor` in combination with our sprites to create a shadow behind each sprite to make them really stand out in the game. This is done by drawing our sprites into a layer after applying the shadow processor.

Let's get started by creating our layer class: