
```
Notification notification =
    new Notification('My Notification', body: "Hello World");
}
});
}
```

Before you send any notifications to the user, the website must have permissions. You can let websites send a notification automatically or by means of a permission request first. This is a common requirement when an API tries to interact with something outside a web context, especially to avoid sending useless notifications to the user. To see the notification settings, follow these steps:

1. Open the **Settings** option in Dartium and type `notifications` in the search field, as shown in the following screenshot:

    ![](img/52c05b53a56abd53584e4fed738ed509_487_0.png)

2. Open the **Content** settings pop-up dialog by clicking on the button of the same name, and then scroll down to find the **Notification** settings:

    ![](img/52c05b53a56abd53584e4fed738ed509_487_1.png)

3. Choose the recommended option and open the **Notifications exceptions** dialog by clicking on the **Manage exceptions** button. For now, it will not contain our website, as shown in the following screenshot:

    ![](img/52c05b53a56abd53584e4fed738ed509_488_0.png)

4. Now, run the application and click on the **Notify** button. As shown in the following screenshot, the `requestPermission` static method of the **Notification** class requests a permission to allow desktop notifications for the `localhost`:

    ![](img/52c05b53a56abd53584e4fed738ed509_488_1.png)

5. You can allow or deny notifications for this website. Your choice will complete the future permission requests with the value of the chosen permission. The desktop notifications that are allowed are added to the list of **Notifications exceptions**. Now, open the **Notifications exceptions** dialog again to see your website, as shown in the following screenshot:

    ![](img/52c05b53a56abd53584e4fed738ed509_489_0.png)

6. The next step is to create a notification. It is enough to specify only the title of the notification to create an original one. The constructor of the `Notification` class has optional properties that help us create notifications with a body and icon. A notification when instantiated is displayed as soon as possible, as shown in the following screenshot:

    ![](img/52c05b53a56abd53584e4fed738ed509_489_1.png)

A notification triggers the following four events that track the current state of the notification:

- `show`: This event is triggered when the notification is displayed
- `click`: This event is triggered when the user clicks on the notification
- `close`: This event is triggered when the notification is closed
- `error`: This event is triggered when something goes wrong while displaying notifications

## Supporting Other HTML5 Features

Notifications are still open until the user closes them, but you can use the `close` method of the `Notification` class to close them via a program, as shown in the following code:

```
void sendNotification() {
    Notification.requestPermission().then((String permission) {
        if (permission == "granted") {
            Notification notification =
                new Notification('My Notification', body: "Hello World");
            notification.onShow.listen((Event event) {
                new Timer(new Duration(seconds:2), () {
                    notification.close();
                });
            });
        }
    });
}
```

## Preventing repeated notifications

Every time you click on the Notify button in your application, the web notification API generates new notifications and puts them on top of the previous one. Similar notifications can be marked with the following tag attribute to prevent crowding a user's desktop with hundreds of analogous notifications:

```
void sendNotification() {
    Notification.requestPermission().then((String permission) {
        if (permission == "granted") {
            Notification notification =
                new Notification('My Notification', body: "Hello World",
                    tag: "myNotification");
            notification.onShow.listen((Event event) {
                new Timer(new Duration(seconds:2), () {
                    notification.close();
                });
            });
        }
    });
}
```

Now when you generate a notification with the same tag, the web notification API removes the previous one and adds the new one instead. Web notification specifications are not stable yet, and they are supported only by the latest version of web browsers such as Chrome, Firefox, Safari, and Opera.

## The native drag-and-drop APIs

**Drag-and-drop** is a way to convert the pointing device's movements and clicks to special commands that are recognized by software to provide quick access to common functions of a program. The user grabs a virtual object, drags it to a different location or another virtual object, and drops it there. Drag-and-drop support for a native browser means faster, more responsive, and more intuitive web applications. Before you use the drag-and-drop feature, make sure you have draggable content.

## Draggable content

An abstract `Element` class has a `draggable` attribute that indicates whether the element can be dragged and dropped. As all the DOM elements emerge from the `Element` abstract class, this means all of them support the drag-and-drop operation by default. To make elements draggable, we need to set their `draggable` attribute to `true`. This can be done using the following code:

```
var dragSource = querySelector("#sample_drag_id");
dragSource.draggable = true;
```

Alternatively, you can do this using the following HTML markup:

```
<p id="sample_dnd_id" draggable="true">Drag me!</p>
```

If you want to prevent the text contents of draggable elements from being selected, you can style the element, as shown in the following code:

```
[draggable] {
  -moz-user-select: none;
  -khtml-user-select: none;
  -webkit-user-select: none;
  user-select: none;
  -khtml-user-drag: element;
  -webkit-user-drag: element;
}
```

Let's open the `drag_and_drop` project and run it. In the following screenshot, you will see that you can drag the text element within the window of the browser but cannot drop it:

![](img/52c05b53a56abd53584e4fed738ed509_492_0.png)

> During a drag operation, the native drag-and-drop API is fired only by the drag events and not the mouse events.

## The drag-and-drop events

The native drag-and-drop API fires the following events:

- `dragstart`: This event is fired on an element when a drag starts. Information such as the drag data and image to be associated with the drag operation can be set in the listener.
- `dragenter`: This event is fired when the cursor is hovered over an element for the first time while a drag begins. A drop operation is not allowed by default. There are one or more listeners that perform drag-and-drop operations. Usually, the listener highlights or marks the drop element where the drop can occur.
- `dragover`: This event is fired when the cursor is hovered over an element and a drag is in process.
- `dragleave`: This event is fired when the cursor leaves an element while a drag is in process. The listener will remove highlights or markers from the element where the drop can occur.
- `drag`: This event is fired on an element where the `dragstart` event was fired.
- **drop**: This event is fired on an element where the drop occurred. It is fired only if the drop is allowed. Users can cancel the drop operation by pressing the *Esc* key or releasing the mouse button on an invalid drop area.
- **dragend**: This event is fired on an element on which the drag was started to inform that the drag operation is complete, regardless of whether it is successful or not.

We will continue to make elements draggable from our example. We need to add a listener for the `dragstart` event and set the drag data within the listener, as follows:

```
var dragSource = querySelector("#sample_drag_id")
..draggable = true
..onDragStart.listen((MouseEvent event) {
  //...
});
```

If an element is made draggable, you cannot select the text by clicking-and-dragging with the mouse.

![](img/52c05b53a56abd53584e4fed738ed509_493_0.png)

> User must hold down the Alt key to select text with the mouse.

## Dragging data

Each drag event has a `dataTransfer` property that is used to hold data associated with the drag operation. If you drag the selected text, then the associated data is text. If you drag an image, then the associated data is the image itself. The drag data combines the string representation of the format of the data and the data value. We will use the format of the data in the event listeners for the `dragenter` and `dragover` events to check whether the drop operation is allowed or not. You can set multiple drag data to call the `setData` method multiple times with different formats. To delete them, call the `clearData` method of the `dataTransfer` property, as shown in the following code:

```
var dragSource = querySelector("#sample_dnd_id")
..draggable = true
..onDragStart.listen((MouseEvent event) {
  event.dataTransfer.setData("text/plain", "I'm draggable");
  event.dataTransfer.setData("text/data", "1234");
});
```
