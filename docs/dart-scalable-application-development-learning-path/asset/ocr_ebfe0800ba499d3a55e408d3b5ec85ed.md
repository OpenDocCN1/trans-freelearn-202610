+   - **drop**: This event is fired on an element where the drop occurred. It is fired only if the drop is allowed. Users can cancel the drop operation by pressing the *Esc* key or releasing the mouse button on an invalid drop area.
+   - **dragend**: This event is fired on an element on which the drag was started to inform that the drag operation is complete, regardless of whether it is successful or not.

We will continue to make elements draggable from our example. We need to add a listener for the `dragstart` event and set the drag data within the listener, as follows:

```
var dragSource = querySelector("#sample_drag_id")
..draggable = true
..onDragStart.listen((MouseEvent event) {
  //...
});
```

If an element is made draggable, you cannot select the text by clicking-and-dragging with the mouse.

![](bbox=[0.22, 0.436, 0.318, 0.477])

> User must hold down the Alt key to select text with the mouse.

# Dragging data

Each drag event has a `dataTransfer` property that is used to hold data associated with the drag operation. If you drag the selected text, then the associated data is text. If you drag an image, then the associated data is the image itself. The drag data combines the string representation of the format of the data and the data value. We will use the format of the data in the event listeners for the `dragenter` and `dragover` events to check whether the drop operation is allowed or not. You can set multiple drag data to call the `setData` method multiple times with different formats. To delete them, call the `clearData` method of the `dataTransfer` property, as shown in the following code:

```
var dragSource = querySelector("#sample_dnd_id")
..draggable = true
..onDragStart.listen((MouseEvent event) {
  event.dataTransfer.setData("text/plain", "I'm draggable");
  event.dataTransfer.setData("text/data", "1234");
});
```