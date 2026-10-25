# User options

We now have a reusable set of shape types, a set of drawing types that contain references to our shapes, an engine to control execution flow, and an object to keep track of and manage the drawing objects. The only thing we are missing is user options to manipulate what action is taken. In this section, we will be modifying our original HTML and finishing the implementation of the DrawingModel type.

The first thing we need to do is add some new buttons. Each one will be associated with a different tool type. As you can see in the following screenshot, we now have three new buttons, each of which displays the name of the tool that it represents:

![](bbox=[0.06, 0.265, 0.615, 0.61])

Now that we have the buttons, we need to perform some work when they are clicked. The DrawingModel will handle this work for us since we will need to modify properties of this object directly. We will need to add an event handler for each button that sets a private member of the model to the corresponding drawing tool. In the example of this class earlier, we had an empty constructor; we need to change this so that when the model is created, it starts listening for the click events.

```
private _drawingTool: DrawingToolType = DrawingToolType.Select;
constructor() {
    this._addEventListeners();
}
private _addEventListeners() {
    var selectButton = window.document.getElementById("selectButton");
    selectButton.addEventListener("click", (e) => {
        this._drawingTool = DrawingToolType.Select;
    }, true);
```