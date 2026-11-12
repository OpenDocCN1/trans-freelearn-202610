1. Open the `effects.dart` file and below the `explodingParticle` function we added earlier in `Step 2` of the `Animating with particles` section, add the following class definition for our layer:

```
class ShadowLayer extends DynamicLayer {
  final Function renderFunction;
  ShadowLayer(this.renderFunction) {
    preProcessors.add(ShadowProcessor(color:
      Colors.black, offset: const Offset(4, 4)));
  }

  @override
  void drawLayer() {
    renderFunction(canvas);
  }
}
```

Here, we create a class called `ShadowLayer`, which is a `DynamicLayer`. In the constructor, we add the shadow processor to the list of preprocessors so that the shadow effect gets applied before drawing the sprites. Note that this is a list of preprocessors, so if you do create your own, you can add multiple effects to your layers. Also, there is a postprocessors list available too, which adds effects after your sprites are drawn if you need it.

We set the shadow to be black and drawn at an offset 4 , 4 pixels away from where the sprite is drawn to give the effect of a shadow behind the sprite.

In the constructor, we pass a function reference that is then used when we call `drawLayer()`. We do this because we want to hold a reference to the super class render function, so that we delegate the drawing to the layer. So, our sprites will draw onto the layer where they will have their shadow applied, and then we call on the super class to draw the layer to the game, based on whatever animation frame we are currently rendering.

Because of this, when we render our sprites in the render function, we will render into `ShadowLayer` and not make a call to the super class there, or we will be drawing twice, which is inefficient and not needed.

2. Now, at the top of the file, let's add an import for the layer package:

```
import 'package:flame/layers.dart';
```