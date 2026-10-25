# # Kendo UI TreeView

As the user drags the node, various events are generated. The application can listen to these events and customize its behavior. The drag, dragstart, dragend, and drop events are the set of events for which the user can specify the event listeners.

Let's consider the drag event. The generated event contains meta information and includes a function to set the tooltip, that is, a tooltip is shown when the user is performing the drag operation. The event contains properties such as sourceNode, which refers to the element being dragged; dropTarget, which refers to the element that the sourceNode is placed over; pageX and pageY, which refer to the x and y coordinates of the mouse pointer on the screen; and statusClass, which indicates the status of the drag operation, that is, the node being added, inserted, or denied if the operation is not acceptable.

To set the tooltip, invoke the setStatusClass function on the event object. This function accepts a string as a parameter, which should be one of k-insert-top, k-insert-middle, k-insert-bottom, k-add, or k-denied.

The statusClass property can be used in the setStatusClass function to set the appropriate tooltip while the element is being dragged:

```
```
$(\'#treeView\').kendoTreeView({
    dragAndDrop: true,
    drag: function (e) {
        e.setStatusClass(\'k-\' + e.statusClass);
    }
});
```
```

Now, when the element is being dragged, the tooltip is set by invoking the setStatusClass function. The status of the drag operation is available in the statusClass property of the event object.

# ## How it works...

After setting the dragAndDrop property to true, the elements in TreeView can be dragged-and-dropped to any level in the tree.

![](bbox=[0.377, 0.706, 0.644, 0.818])

The previous screenshot indicates that the File1.txt element is being dragged and the tooltip indicates that the element will be inserted at the bottom, that is, it would be the last element under the parent node, Directory1. When the element is being dragged, the statusClass property is set to insert-bottom and the tooltip is set to k-insert-bottom.