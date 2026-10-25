

## Kendo UI Window

Now, when the page loads, the modal window will be hidden. Add a `click` event handler for the button that will open the modal window:

```javascript
$('#btn1').on('click', function() {
    var modalObject = $('#window').data('kendoWindow');
    modalObject.open();
});
```

The first line gets the instance of the modal window. Then, in the next line, the open method is called on the window instance that would open the modal window.

The `open` method is one of the several methods that can be used to act on the window object. We will take a look at other methods in the following recipe.

## Loading the content of the modal using Ajax

In this recipe, the content of the modal was specified in the HTML markup. The content of the modal window can be loaded by specifying the URL that returns the HTML markup. This content will be fetched asynchronously when the page is loaded.

To load the content of the window, specify the content attribute when initializing the window using `kendoWindow`:

```javascript
$('#window').kendoWindow({
    visible: false,
    modal: true,
    content: './jsbooks.html'
});
```

Here, the `./jsbooks.html` URL is specified in the `content` attribute. This can be any URL that returns the HTML markup. For example, it could be a PHP page that generates the HTML markup. When you load the page, an asynchronous request is sent to `./jsbooks.html`, and the generated markup is inserted into the window.

## Customizing the look and feel of the window and including action buttons

There are various options available to the user to customize the window object. In this recipe, we will take a look at these options.

### How to do it...

Some of the customization options for the window have a default value. For example, the pop-up window can be resized and is draggable. Also, the window is shown since the visible property is set to true. Let's take a look at some of the other properties:

```javascript
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

The title bar of the window can be customized to add more action buttons. By default, only the close (x) button is shown. The action buttons that can be added to the title bar include Close (added by default), Minimize, Maximize, Refresh, and Pin. These options can be specified as the value to the actions attribute:

```javascript
$('#window').kendoWindow({
    visible: false,
    modal: true,
    title: "Example Modal Window",
    actions: ['Pin', 'Refresh', 'Minimize', 'Maximize', 'Close'],
    width: '400px',
    height: '300px',
    content: './jsbooks.html'
});
```

### How it works...

When you execute the preceding code snippet, you will see that the window has a title bar and various action buttons, as shown in the following screenshot:

![](img/2a66507445090790d34dc5588b56b24c_117_0.png)

The action buttons are added in the same order as they are specified. When you click on the Refresh button, a request will be sent to the URL specified in the content attribute. If the content of the window is specified inline, that is, in the same page, then the refresh button is not required.

When you click on the Minimize or Maximize button, the window is adjusted such that only the title is shown or expanded to occupy the entire screen, respectively. In both scenarios, these buttons are replaced with a Restore button. On clicking on this button, the window is restored to its original state.

The Pin button allows you to fix the position of the window, that is, when the user scrolls, the window continues to be in the same fixed position. Also, if the window is pinned, the icon in the title bar is replaced with the Unpin button so that the user can restore the window to its original state.

### There’s more...

It is also possible to add a custom action button in the title bar of the window instance. Also, the action to perform when the custom button is clicked on can also be defined by adding a click event handler.

To add a custom action button to the title bar, first specify Custom in the actions array:

```javascript
$('#window').kendoWindow({
    title: "Example Modal Window",
    actions: ['Custom', 'Pin', 'Refresh',
        'Minimize', 'Maximize', 'Close'],
    width: '400px',
    height: '300px',
    content: './jsbooks.html'
});
```

The next step is to define an event handler for the custom button added to the title bar:

```javascript
var modalObject = $('#window').data('kendoWindow');
modalObject.wrapper.find('.k-i-custom').on('click', function() {
    alert(modalObject.options.title);
});
```

The preceding code first gets an instance of the window and then adds a click handler for the custom button. Here, a reference to the custom icon is used, that is, the k-i-custom class to add a click handler.

When you execute the page, you will see the custom icon being shown in the title bar.

![](img/2a66507445090790d34dc5588b56b24c_118_0.png)

In the preceding screenshot, the first icon from the left is the custom icon. When you click on the custom icon, an alert message that displays the title of the window is shown. For a given window, you can add only one custom icon.

## Using the Window API to act on a window object

In this recipe, we will look at several methods that can be used to act on the window object.

### How to do it...

There are various methods available that help you act on an existing window object. These include the following methods:

- **center**: This will center the window.
- **open**: This will open the window.
- **close**: This will close the window.
- **refresh**: This will refresh the content of window. Optionally, some parameters can be passed to this function, which will override the default options.
- **maximize**: This will maximize the window to the entire viewing area.
- **minimize**: This will minimize the window to its title bar.
- **restore**: This will restore the window from its minimized or maximized state.
- **pin**: This will pin the window. It will fix the window's position, and hence, when you scroll, the window will appear at the same position.
- **unpin**: This will unpin the window and remove the fixed position.
- **toggleMaximization**: This toggles the window between the maximized and restored state.

Let's test these functions by creating a window and adding buttons that will execute the corresponding actions when clicking on them:

The following is HTML markup that contains buttons:

```html
<button id="btnOpen">Open</button><br>
<button id="btnClose">Close</button><br>
<button id="btnRefresh">Refresh</button><br>
<button id="btnMaximize">Maximize</button><br>
<button id="btnToggleMaximization">Toggle Maximization</button><br>
<button id="btnMinimize">Minimize</button><br>
<button id="btnRestore">Restore</button><br>
<button id="btnPin">Pin</button><br>
<button id="btnUnpin">Unpin</button><br>
```

Now, initialize the window object as follows:

```javascript
$('#window').kendoWindow({
    visible: true,
    title: 'Example Modal Window',
    width: '400px',
    height: '300px',
    content: './jsbooks.html',
    position: {
        top: '200px',
        left: '500px'
    }
});
```

After initializing the window, add the onclick event listeners as follows:

```javascript
var modalObject = $('#window').data('kendoWindow');

$('#btnOpen').on('click', function() {
    modalObject.open();
});

$('#btnClose').on('click', function() {
    modalObject.close();
});

$('#btnRefresh').on('click', function() {
    modalObject.refresh();
});

$('#btnMaximize').on('click', function() {
    modalObject.maximize();
});

$('#btnToggleMaximization').on('click', function() {
    modalObject.toggleMaximization();

    //restore to original state after 2 seconds
    setTimeout(function () {
        modalObject.toggleMaximization()
    }, 2000);
});

$('#btnMinimize').on('click', function() {
    modalObject.minimize();
});

$('#btnRestore').on('click', function() {
