Hopefully, this can help you in improving your geographical knowledge and learning more about Dart!

When debugging in Dartium using a REST or other similar web service, it is possible to get extra logging in the console window of the browser's developer tools for XMLHttpRequests.

Bring up the context menu (right-click), and there is an option named Log XMLHttpRequests that displays the HTTP requests made by the web page in the console output window:

[-121.5371704, 36.8165016, 0.93]
129.91739911111114 177.278328
2.2222222222222223 3.3333333333333335
maxWidth 2
XHR finished loading: GET "http://127.0.0.1:8080/api/quake/v1/latest".

This can be invaluable when debugging a project and saves adding logging to every part of the code that makes an HTTP request.

# Zooming into the display

The canvas element has a range of features for 2D drawing, and this includes a scaling feature that we can use to hone in on the most active part of the map, North America. In main.dart, a flag exists called zoomed, defaulting to false, that aids the application in keeping track of the current display. A button is bound to the zoom code implementation:

```
zoomMap(Event evt) async {
  if (zoomed == true) {
    zoomed = false;
    quakeMap.mapCtx.resetTransform();
    quakeMap.mapCtx.scale(1, 1);
  } else {
    zoomed = true;
    quakeMap.mapCtx.scale(2, 2);
  }
  quakeMap.drawMapGrid();
}
```