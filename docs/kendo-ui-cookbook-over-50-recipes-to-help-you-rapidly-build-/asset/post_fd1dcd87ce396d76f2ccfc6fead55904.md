
{ text: "File1.txt" },
{ text: "File2.txt" }
],
expanded: true
}, {
text: "Directory2",
items: [
{ text: "File1.txt" },
{ text: "File2.txt" }
]},
{
text: "Directory3",
items: [
{ text: "File1.txt" },
{ text: "File2.txt" }
]
}
]
});
```

Here, the DataSource object is an array of objects and each object specifies the properties for a node in the TreeView widget. Each object has a text attribute, which specifies the label for the node in the TreeView widget. It also has an items collection, which specifies the subitems for the node in TreeView.

The first object in the DataSource collection has an expanded attribute set to true to indicate that the node would be expanded to show subitems.

## Constructing a TreeView widget from a remote DataSource object

It is also possible to construct a TreeView widget by fetching the data from a remote service. The DataSource object can specify the URL for the remote service from where the hierarchical data would be fetched:

```javascript
$('#treeView').kendoTreeView({
dataSource: {
transport: {
read: '/services/music'
}
}
});
```

Here, the remote service URL, /services/music, should return the data that contains first-level elements in the TreeView widget. Let's assume that the service returns the following data:

```json
[
    {
        "categoryId": 1,
        "categoryName": "Genre",
        "hasSubCategories": true
    },
    {
        "categoryId": 2,
        "categoryName": "Artists",
        "hasSubCategories": true
    },
    {
        "categoryId": 3,
        "categoryName": "Songs",
        "hasSubCategories": true
    },
    {
        "categoryId": 4,
        "categoryName": "Playlists",
        "hasSubCategories": true
    }
]
```

The preceding JSON response from the service returns four records and these are top-level nodes in the TreeView widget. Each object in the response contains three fields; when initializing the TreeView widget, the field that should be used to display the text should be specified:

```javascript
$('#treeView').kendoTreeView({
    dataSource: {
        transport: {
            read: '/services/music'
        }
    },
    dataTextField: 'categoryName'
});
```

## Kendo UI TreeView

Now, when you execute the previous code, you would see the category names being displayed in the TreeView widget. Here, the `dataTextField` attribute is used to specify the field name that should be used when displaying labels in the TreeView widget.

To populate the next level of nodes in TreeView, the widget should be instructed whether the current node contains subitems or not. Notice that the response contains the `categoryId` and `hasSubCategories` fields; the `categoryId` field serves as a unique ID for each object in the response and the `hasSubCategories` field indicates whether the object contains subitems. This should be identified in the `DataSource` object and hence a schema for the same should be specified:

```javascript
$('#treeView').kendoTreeView({
    dataSource: {
        transport: {
            read: 'TreeView.json'
        },
        schema: {
            model: {
                id: 'categoryId',
                hasChildren: 'hasSubCategories'
            }
        }
    },
    dataTextField: "categoryName",
});
```

Here, the properties in the schema models, `id` and `hasChildren`, are mapped to the attributes in the response mentioned earlier. Now, TreeView shows you the list of top-level nodes and indicates that each node in the tree has children. When you click on any of the nodes in TreeView, a `GET` request is sent to remote services, specifying the `categoryId` as a query parameter. For example, when you click on the first node in the tree, a `GET` request is sent with the `categoryId` query parameter set to 1:

```
/services/music?categoryId=1
```

The service is expected to return a similar JSON response, that is, a collection of objects, each containing the `categoryId` and `hasSubCategories` attributes.

## Using the drag and drop feature to move elements in TreeView

In a TreeView widget, elements in the tree can be dragged-and-dropped to various levels. This is applicable not only to child elements, but also to top-level nodes in the tree.

### How to do it...

To enable the drag and drop feature, specify the dragAndDrop property as true while initializing the TreeView widget, as shown in the following code snippet:

```javascript
$('#treeView').kendoTreeView({
dragAndDrop: true,

dataSource: [{
text: "Directory1",
items: [
{ text: "File1.txt" },
{ text: "File2.txt" }
],
expanded: true
}, {
text: "Directory2",
items: [
{ text: "File3.txt" },
{ text: "File4.txt" }
] }, {
text: "Directory3",
items: [
{ text: "File5.txt" },
{ text: "File6.txt" }
]
}
]
});
```

Here, when the dragAndDrop property is set to true, the elements in TreeView can be moved between various levels in the tree.

## Kendo UI TreeView

As the user drags the node, various events are generated. The application can listen to these events and customize its behavior. The drag, dragstart, dragend, and drop events are the set of events for which the user can specify the event listeners.

Let's consider the drag event. The generated event contains meta information and includes a function to set the tooltip, that is, a tooltip is shown when the user is performing the drag operation. The event contains properties such as sourceNode, which refers to the element being dragged; dropTarget, which refers to the element that the sourceNode is placed over; pageX and pageY, which refer to the x and y coordinates of the mouse pointer on the screen; and statusClass, which indicates the status of the drag operation, that is, the node being added, inserted, or denied if the operation is not acceptable.

To set the tooltip, invoke the setStatusClass function on the event object. This function accepts a string as a parameter, which should be one of k-insert-top, k-insert-middle, k-insert-bottom, k-add, or k-denied.

The statusClass property can be used in the setStatusClass function to set the appropriate tooltip while the element is being dragged:

```javascript
$('#treeView').kendoTreeView({
    dragAndDrop: true,
    drag: function (e) {
        e.setStatusClass('k-' + e.statusClass);
    }
});
```

Now, when the element is being dragged, the tooltip is set by invoking the setStatusClass function. The status of the drag operation is available in the statusClass property of the event object.

### How it works...

After setting the dragAndDrop property to true, the elements in TreeView can be dragged-and-dropped to any level in the tree.

![](img/2a66507445090790d34dc5588b56b24c_67_0.png)

The previous screenshot indicates that the File1.txt element is being dragged and the tooltip indicates that the element will be inserted at the bottom, that is, it would be the last element under the parent node, Directory1. When the element is being dragged, the statusClass property is set to insert-bottom and the tooltip is set to k-insert-bottom.

Similarly, when the element is moved to a different node, you would notice that the tooltip changes to indicate that the dragged element (sourceNode) would be added to a node in TreeView.

![](img/2a66507445090790d34dc5588b56b24c_68_0.png)

Notice the tooltip in the preceding screenshot; it indicates that the element is being added to Directory3. After dropping the element, the parent node is expanded to show its content.

![](img/2a66507445090790d34dc5588b56b24c_68_1.png)

### There’s more...

In many scenarios, you would like one of the nodes in the tree to not accept a drop from any other element. In this case, the drop event listener can be used to prevent a drop from any node in the tree:

```javascript
$('#treeView').kendoTreeView({
 dragAndDrop: true,
 drop: function(e) {
 if ($(e.dropTarget)[0].innerText == 'Directory3') {
 e.setValid(false);
 }
 }
});
```

Here, the dropTarget node is checked to see whether it contains the Directory3 text.

If this condition is true, then the setValid function is invoked on the event object, passing false as the parameter. This will disable the drop of any sourceNode over the node that contains the Directory3 text.

In another scenario, if you would like to disable the dragging of top-level nodes in the tree, that is, the root nodes of the tree, then an event listener for the dragstart event can be added:

```javascript
$('#treeView').kendoTreeView({
dragAndDrop: true,

dragstart: function (e) {
if($(e.sourceNode).parentsUntil('.k-treeview', '.k-item').length == 0) {
e.preventDefault();
}
}
});
```

Here, sourceNode is checked for parent-level nodes. If one is not found, that is, if sourceNode itself is the parent node, then by calling the preventDefault function on the event object, the action of dragging the element is disabled.

## Using checkboxes to select nodes in TreeView

In TreeView, each node can have a checkbox associated with it. By using a checkbox, you can select multiple nodes in the tree and perform an action.

### How to do it...

To add a checkbox next to a node in TreeView, specify the checkboxes attribute as true when initializing the TreeView widget:

```javascript
$('#treeView').kendoTreeView({
checkboxes: true
});
```

If you want the child nodes to be selected when the parent node is selected, then specify checkChildren as true:

```javascript
$('#treeView').kendoTreeView({
checkboxes: {
