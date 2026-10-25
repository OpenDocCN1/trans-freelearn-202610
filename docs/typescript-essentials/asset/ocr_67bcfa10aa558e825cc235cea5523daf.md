```
this._drawingColor = (<any>e.currentTarget).value;
}, true);
}
public getNewShape(location: IPoint): IDrawingShape {
    var shape: IDrawingShape = null;
    var cursor: string = "auto";
    switch (this._drawingTool) {
        case DrawingToolType.Rectangle:
            shape = new DrawingRectangle();
            shape.Move(location);
            (<DrawingRectangle>shape).shape.height = 3;
            (<DrawingRectangle>shape).shape.width = 3;
            (<DrawingRectangle>shape).fillStyle = this._drawingColor;
            cursor = "se-resize";
            break;
        case DrawingToolType.Line:
            shape = new DrawingLine();
            (<DrawingLine>shape).shape.p1 = location;
            (<DrawingLine>shape).shape.p2 = new Point(location.x + 1, location.y + 1);
            (<DrawingLine>shape).strokeStyle = this._drawingColor;
            cursor = "e-resize";
            break;
    }
    window.document.body.style.cursor = cursor;
    return shape;
}
```

As you can see, we added a new private variable to store the current color value. The extra event handler was added to listen for the color picker’s value to change and to store this value. The getNewShape code simply sets the appropriate property for the type being instantiated and the shape object handles the rest. As you can see in the following screenshot, we can now create objects of any size or color: