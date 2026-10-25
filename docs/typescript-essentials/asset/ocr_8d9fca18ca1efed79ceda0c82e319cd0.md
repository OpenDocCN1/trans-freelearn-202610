# # The engine

To handle the drawing and updating of the canvas state, we will need a new type that is focused around maintaining this. This type, which we’ll call `CanvasEngine`, should have a very simple abstraction since it only needs to perform a few basic tasks: drawing the canvas, clearing the canvas, and receiving requests to redraw the current frame:

```
interface ICanvasEngine {
    invalidate();
    clear();
    draw();
}
```

The object will do more under the covers, but any object consuming the engine needs only these functions to interact with it. Now let’s take a look at the more complex implementation of this type. In the following screenshot, you can see the full implementation. This will be followed by a breakdown of each public and private method.

![](bbox=[0.06, 0.36, 0.84, 0.63])

As you can see, we have a number of private methods and event handlers that will help us interpret user actions. The constructor takes in the canvas we want to work with and a model that is responsible for maintaining a list of shapes for the engine to operate on. This model has been abstracted the same way the rest of our types have to maintain a decoupled application.

```
interface IDrawingModel {
    selection: IDrawingShape;
    shapes: IDrawingShape[];
    addShape(shape: IDrawingShape);
    getNewShape(location: IPoint): IDrawingShape;
    getDrawingTool(): DrawingToolType;
}
```

We will look at the concrete implementation of this type later, but knowing what the