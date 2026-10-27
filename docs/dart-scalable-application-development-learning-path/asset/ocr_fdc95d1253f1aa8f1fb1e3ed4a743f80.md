# A Real-Time Visualization

The canvas is toggled between scaling settings, with a critical call to the method `resetTransform`. Without this, the scaling would not be back to normal. The entire map view must be redrawn immediately so that the user has a responsive experience. Let's have a look at the following screenshot:

![](bbox=[0.176, 0.223, 0.824, 0.621])

If you watch this page for a while, you will find the California and Alaska feature embedded heavily in the data. As the pop-up code handles the change in scale, these are still available. Pressing the **Zoom** button a second time returns to the full world view.

> The print function is not just for command-line applications but can be used in web applications, too. The output appears in the **Developer Tools** console log window, which can be displayed by right-clicking on the page to bring up the context menu and then clicking `Inspect Element`.