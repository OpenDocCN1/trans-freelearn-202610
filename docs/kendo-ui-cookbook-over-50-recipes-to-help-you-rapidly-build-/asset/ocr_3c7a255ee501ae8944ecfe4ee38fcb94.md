# # Customizing the look and feel of the window and including action buttons

There are various options available to the user to customize the window object. In this recipe, we will take a look at these options.

# ## How to do it...

Some of the customization options for the window have a default value. For example, the pop-up window can be resized and is draggable. Also, the window is shown since the visible property is set to true. Let's take a look at some of the other properties:

```
$('#window').kendoWindow({
    visible: false,
    modal: true,
    title: "Example Modal Window",
    width: '400px',
    height: '400px',
    maxWidth: '600px',
    maxHeight: '600px',
    minWidth: '200px',
    minHeight: '200px',
    position: {
        top: '100px',
        left: '100px'
    }
});
```

Here, the title attribute specifies the title of the window. When the modal window is rendered, its width and height would be 400px. This modal window can be resized; however, it can be resized to a maximum width (maxWidth) and height (maxHeight) of 600px and a minimum width (minWidth) and height (minHeight) of 200px. The position attribute specifies the top and left positions as 100px, and the window would be positioned accordingly. If the same is not specified, then the window is positioned in relation to the document by default.