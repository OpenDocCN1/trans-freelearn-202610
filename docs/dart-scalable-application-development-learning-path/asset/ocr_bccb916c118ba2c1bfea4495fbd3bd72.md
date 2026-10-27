context.moveTo(startPoint.x, startPoint.y);
context.lineTo(point.x, point.y);

context.stroke();
context.closePath();
}
}
}

We must always clear the whole drawing canvas and start a new path via the `beginPath` method of context. We move the first point to the `startPoint` position and draw the line within the current cursor coordinates. Finally, we draw the line shape by stroking its outline and close the drawing path.

# The Rectangle tool

We can draw a rectangle with individual lines, but the **Rectangle** tool makes the task much easier. The context object has the following methods to draw rectangles:

+   - `strokeRect`: This method uses the current stroke style to draw the box that outlines the given rectangle onto the canvas
+   - `fillRect`: This method uses the current fill style to draw the given rectangle onto the canvas
+   - `clearRect`: This method clears all the pixels on the canvas in the given rectangle to transparent black

The easiest way to draw a rectangle on the canvas is use the `fillRect` method of context. The `fillRect` method uses color from the `fillStyle` property, black by default. The Rectangle class, as shown in the following code:

```
/*
 * Rectangle tool
 */
class Rectangle extends Tool {

    String get name => "Rectangle";

    @override
    void drawing(CanvasRenderingContext2D context, Point point) {
        if (isDrawing) {
            context.clearRect(0, 0,
                context.canvas.width, context.canvas.height);

            int x = min(point.x, startPoint.x).round(),
```