```
var rectButton = window.document.getElementById("rectangleButton");
rectButton.addEventListener("click", (e) => {
    this._drawingTool = DrawingToolType.Rectangle;
}, true);
var rectButton = window.document.getElementById("lineButton");
rectButton.addEventListener("click", (e) => {
    this._drawingTool = DrawingToolType.Line;
}, true);
}
```

With these event handlers setting the private variable to the appropriate drawing tool, we can now fill in the remainder of this implementation as well. Previously, we left the getNewShape method without a body and the getDrawingTool method always returned the Select tool. Now that we have a private reference to the tool we should be using, getDrawingTool will just return this value. The getNewShape method needs a little more work though. In the following example, you can see that we create the new shape and set some of the initial properties for drawing:

```
public getNewShape(location: IPoint): IDrawingShape {
    var shape: IDrawingShape = null;
    var cursor: string = "auto";
    switch (this._drawingTool) {
        case DrawingToolType.Rectangle:
            shape = new DrawingRectangle();
            shape.move(location);
            (<DrawingRectangle>shape).shape.height = 3;
            (<DrawingRectangle>shape).shape.width = 3;
            cursor = "se-resize";
            break;
        case DrawingToolType.Line:
            shape = new DrawingLine();
            (<DrawingLine>shape).shape.p1 = location;
            (<DrawingLine>shape).shape.p2 = new Point(location.x + 1, location.y + 1);
            cursor = "e-resize";
            break;
    }
    window.document.body.style.cursor = cursor;
    return shape;
}
public getDrawingTool(): DrawingToolType {
    return this._drawingTool;
}
```

Based on the private state of the model, we either create DrawingRectangle or DrawingLine. The full implementation of the DrawingRectangle class has been shown in this text; the implementation of the DrawingLine class is available for download along with all the other code samples in this book. Now we have all of the components necessary to run our application and allow users to create and modify shapes as they please. The final code for app.ts is as follows:

```
var canvas: HTMLCanvasElement = <HTMLCanvasElement>window.document.getElementById("drawingCanvas");
var model: IDrawingModel = new DrawingModel();
```