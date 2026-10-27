```
void drawing(CanvasRenderingContext2D context, Point point);

void finishDraw(CanvasRenderingContext2D context, Point point) {
    if (isDrawing) {
        drawing(context, point);
        isDrawing = false;
    }
}
```

The isDrawing property reflects the status of the drawing operation. The startPoint property simply holds the cursor coordinates relative to the canvas when the user starts drawing on the canvas. The read-only property name returns the name of the tool. This name is used when the tool is added to ToolSelectorWidget. Each tool has three methods, and the whole drawing process is split into the following three phases:

+   - **Start the drawing phase**: The program calls the beginDraw method when a user starts holding down the mouse button. We always switch on the isDrawing property and remember the cursor coordinates in startPoint.
+   - **Drawing phase**: The drawing method is called every time the user moves the cursor. Implementation of this method strongly depends on the tool, so we do not implement it in the abstract class Tool.
+   - **End the drawing phase**: The program invokes the finishDraw method when the user releases the mouse button. In this method, we need to call the drawing method for the last time and switch off isDrawing only if the drawing process has happened.

Now, it's time to look at our tools implementation in detail.

# The Pen tool

As the canvas element doesn't directly support drawing a single point, we use lines instead. Drawing in a canvas is similar to using a virtual pen. At the beginning, we must call beginPath of context where we begin drawing. This method creates a new drawing path, so future drawing commands will be directed to the path and will be used to build the path. We start our path by moving to the startPoint coordinates with the moveTo method of context, as shown in the following code:

```
/**
 * Simple pen tool
 */
class Pen extends Tool {
```