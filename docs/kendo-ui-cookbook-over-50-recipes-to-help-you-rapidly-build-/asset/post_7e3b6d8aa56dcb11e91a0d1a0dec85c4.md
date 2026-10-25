

## Kendo UI TreeView

checkChildren: true
}
});
```

Now, when you select the parent node, all child nodes within the parent node would be selected. It is also possible to specify nodes as checked when they are rendered. In the DataSource configuration, specify checked as true:

```
$('#treeView').kendoTreeView({
 checkboxes: {
 checkChildren: true
 },
 dataSource: [{
 text: "Directory1",
 items: [
 {text: "File1.txt"},
 {text: "File2.txt"}
 ],
 checked: true
 }
 .
 .
 ]
});
```

# How it works...

When you set the checkboxes property as true, you would see a checkbox before each node in TreeView.

![](img/2a66507445090790d34dc5588b56b24c_70_0.png)

When you check any of the parent nodes (for example, `Directory1`), the child nodes will not be selected. When you set the configuration option, `checkChildren`, to `true`, the child level nodes would be selected.

![](img/2a66507445090790d34dc5588b56b24c_71_0.png)

Also, note that when you select any of the child nodes, the parent node would be partially selected.

![](img/2a66507445090790d34dc5588b56b24c_71_1.png)

Here, the `File1.txt` child node is selected and the root node, `Directory1`, is partially selected. When you select all the child nodes, the parent node would be marked as selected.

# There's more...

When you select any of the nodes in the tree, the corresponding element's checked attribute is set to true. This will trigger a change event on the DataSource object associated with TreeView. A change event listener can be used to check which nodes in the tree are selected:

```
var treeViewWidget = $("#treeView").data("kendoTreeView");
treeViewWidget.dataSource.bind("change", function() {
 var treeView = $("#treeView").data("kendoTreeView"),
 nodes = treeView.dataSource.view();
 (function(nodes) {
 for (var i = 0; i < nodes.length; i++) {
 console.log(nodes[i].text + ' is checked: ' +
 (nodes[i].checked ? 'yes': 'no'));
 if(nodes[i].hasChildren) {
 arguments.callee.call(this, nodes[i].children.view());
 }
 }
 })(nodes);
});
```

Here, the code checks whether the node in the tree is checked; then it iterates over its children recursively and checks whether the child nodes are checked. When you check the first node in the tree, you will see the following messages in the console:

```
Directory1 is checked: no
File1.txt is checked: no
File2.txt is checked: no
Directory2 is checked: yes
File3.txt is checked: yes
File4.txt is checked: yes
Directory3 is checked: no
File5.txt is checked: no
File6.txt is checked: no
```

## Associating icons with labels in TreeView

Similar to associating a checkbox with every node in the TreeView widget, an image or an icon can be associated with a node in the tree. This icon would be displayed before the node name.

# How to do it...

Let's first reference the sprite sheet and add CSS classes to the page:

```
<style type="text/css">
#treeView .k-sprite {
    background-image:
        url('../../examples/content/web/treeview/coloricons-
sprite.png');
}
.folder {
    background-position: 0 0
}
.pdf {
    background-position: 0 -32px;
}
.image {
    background-position: 0 -64px;
}
.html {
    background-position: 0 -48px;
}
</style>
```

The next step would be to associate nodes in the tree with the previously mentioned CSS classes. This can be done by adding a spriteCssClass property for each object in the DataSource collection:

```
$('#treeView').kendoTreeView({
    dataSource: [{
        text: "Directory1",
        spriteCssClass: "folder",
        items: [
            { text: "File1.png", spriteCssClass: "image" },
            { text: "File2.html", spriteCssClass: "html" }
        ],
    }, {
    text: "Directory2",
    spriteCssClass: "folder",
    items: [
        { text: "File3.pdf", spriteCssClass: "pdf" },
        { text: "File4.html", spriteCssClass: "html" }
    ]
    }]
});
```

Here, the root level nodes have the spriteCssClass property set to folder and the ones in the items array are set to the corresponding file type.

# How it works...

When you associate the spriteCssClass property, the text node's containers (the span element) class would be set to k-sprite and the class name that you have specified would be the value of spriteCssClass. For example, the root node, Directory1, would have its container element's class set to k-sprite folder.

When you execute the page, you will see the icons being shown before the nodes in the tree.

![](img/2a66507445090790d34dc5588b56b24c_74_0.png)

## 4 Kendo UI Editor

In this chapter, we will cover the following recipes:

- Creating a WYSIWYG Editor using the `kendoEditor` function
- Adding and removing tools in the Editor toolset
- Using the image browser tool to insert images into the Editor

### Introduction

The Kendo UI Editor widget allows you to create rich content using the **WYSIWYG (What-You-See-Is-What-You-Get)** interface. The interface allows you to format the text, create bulleted or numbered lists, and insert tables, hyperlinks, and images. The toolset can also be customized while initializing the Editor widget. It is also possible to create custom tools that can be used in the Editor. In addition to various Editor controls, you can use an image browser to insert images into the Editor.

### Creating a WYSIWYG Editor using the kendoEditor function

An Editor can be created either by using a `textarea` or `div` element. In this recipe, we will create an Editor using a `textarea` element and show you the default tools available for formatting the data.

# How to do it...

Let's first create a textarea element, specifying the number of rows, columns, and width:

```
<textarea id="editor"
    row="20"
    cols="80"
    style="width: 700px;">
</textarea>
```

The next step is to use this textarea element and create an Editor by invoking the
kendoEditor function:

```
$("#editor").kendoEditor();
```

The kendoEditor function is used to initialize textarea as an Editor widget. The Editor will
now have a WYSIWYG interface, allowing the user to format the text.

# How it works...

When you initialize the textarea element as an Editor by invoking the kendoEditor
function, various tools are available in the toolset, as shown in the following screenshot:

![](img/2a66507445090790d34dc5588b56b24c_77_0.png)

The toolset bar is shown at the top of the Editor and it allows a user to format the entered
data. This includes applying different formatting for the entered text, marking the text as bold
or italic, underlining the selected text, aligning the text, creating a bulleted or numbered list,
and adding images and tables.

# Adding and removing tools in the Editor toolset

The toolbar can be customized to include only those set of tools that are required. For example,
you might like to exclude the option of inserting a table from the toolset bar, or you might like to
include other options to format the text, say, changing the font color, highlighting the selected
text, and so on.

### How to do it...

When initializing the Editor widget, specify the tools option and include only those tools that
are required to be displayed in the toolbar:

```
$("#editor").kendoEditor({
    tools: [
        "bold",
        "italic",
        "strikethrough",
        "subscript",
        "superscript",
        "foreColor",
        "backColor"
    ]
});
```

In the preceding code snippet, the tools that should be displayed in the toolbar are
mentioned. These would replace the default ones.

> The tools configuration option can include the Editor commands such
> as bold, italic, underline, strikethrough, subscript,
> superscript, fontName, fontSize, foreColor, backColor,
> justifyLeft, justifyCenter, justifyRight, justifyFull,
> insertUnorderedList, insertOrderedList, indent, outdent,
> createLink, unlink, insertImage, createTable, addColumnLeft,
> addColumnRight, addRowAbove, addRowBelow, deleteRow,
> deleteColumn, formatting, insertHtml, and viewHtml.

