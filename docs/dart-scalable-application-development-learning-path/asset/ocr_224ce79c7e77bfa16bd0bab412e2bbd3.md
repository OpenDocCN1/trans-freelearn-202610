# How to preview the context

If you need to preview the result of what you've done in the view canvas, you can open a new window with the content of the view canvas as shown here:

```
// Register a preview button listener
querySelector(".preview-btn").onClick
.listen((MouseEvent event) {
    event.preventDefault();
    window.open(canvas.viewCanvas.toDataUrl("image/png"),
    "Image Preview");
});
```

Now if you click on the Preview link, you will see the next result in a new window in your web browser, as shown in the following screenshot:

![](bbox=[0.213, 0.374, 0.787, 0.683])

Here, we opened a new browser window with the image data URI directly and we could save it in the represented format. The data URI format shown in the address of the web browser is as follows:

```
data:[<MIME-type>][;charset=<encoding>][;base64],<data>
```