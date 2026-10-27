```
.onDragStart.listen((MouseEvent event) {
    event.dataTransfer.setData("text/plain", "I'm draggable");
    event.dataTransfer.setDragImage(new
        ImageElement(src:'notification.png'), 0, 0);
    event.dataTransfer.effectAllowed = 'copy';
});
```

In the preceding example, we allowed only the copy operation. Let's see all the values that we can use as the name for an operation:

+   - none: This operation means that no operation is permitted
+   - copy: This operation means that the drag data can only be copied from source to drop location
+   - move: This operation means that the drag data can only be moved from source to drop location
+   - link: This operation means that the drag data can only be linked from source to drop location
+   - copyMove: This operation means that the drag data can be copied or moved from source to drop location
+   - copyLink: This operation means that the drag data can be copied or linked from source to drop location
+   - all: This operation means that the drag data can be copied, moved, or linked from source to drop location

By default, the effectAllowed property allows all three operations. The permitted operation can be checked in a listener for the dragenter or dragover events via the effectAllowed property, and it should be set in a related dropEffect property to specify which single operation should be performed. The valid operations for the dropEffect property are none, move, or link only, and any other combinations are prohibited. The desired operation will change the mouse pointer, so the cursor might appear with a plus for the copy operation. The desired effect can be modified by a user by pressing the modifier keys. The exact keys vary by platform. On Windows OS, a user typically uses the Shift and Ctrl keys to switch between the copy, move, and link operations. During the dragenter and dragover events, we can modify both the effectAllowed and dropEffect properties to specify the supported operations by a drop target. The effect specified in dropEffect must be the one that is listed within the effectAllowed property. The value of the dropEffect property can tell us exactly the result of the drag operation. If the value of the dropEffect property is none, then the drag was cancelled; otherwise, the specified effect holds the performed operation. The drag-and-drop operation is considered complete after the dragexvent is finished.