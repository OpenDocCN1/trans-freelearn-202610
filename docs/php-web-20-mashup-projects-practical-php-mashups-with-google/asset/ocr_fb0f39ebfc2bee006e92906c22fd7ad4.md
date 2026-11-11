# Events

In the Google Maps API Class References documentation, notice that some objects have events associated with them. These objects are things the user sees and can interact with, like the map itself, lines, and markers. This allows you to fire off JavaScript functions whenever the user does something.

Events are managed by the GEvent namespace. To register an event, you must add it to the GEvent object using the addListener() method. addListener() takes three parameters. First, it takes the object that you want the event to be active. Second, it takes the kind of event (click, drag, etc.) that is available on the object. Finally, it takes a handler function that fires when the event is triggered.

Let's add an event to our marker. Adding a few more lines to our callback function, we can add an alert box that pops up when our marker is clicked.

```
function centerMapCallback(returnedPoint) {
    var marker = new GMarker(returnedPoint);
    g_map.setCenter(returnedPoint, 14);
    g_map.addOverlay(marker);
    GEvent.addListener(marker, "click", function() {
        alert("Marker clicked!");
    });
}
```

GEvent is not an object that we create, so we do not need to instantiate it. It is automatically instantiated when we load the Google Maps API. When the click event is triggered on marker, the handler function is executed.

# InfoWindow Box

An alert box is pretty bland. What's more useful is the white popup box that often appears when using Google Maps. These popup boxes look like comic book speech balloons. They point to a specific location on the map, and contain helpful information about that location. In the Google Maps API, these boxes are known as InfoWindows.

InfoWindows are represented in the API by the GInfoWindow class. The most important thing to know about InfoWindows is that for each Google map, there is one and only one InfoWindow. This has two implications to us. First, when the InfoWindow comes and goes from the user's view, all that is happening is that visibility of InfoWindow is being toggled. This is done either through built-in events of the API like, like clicking on the InfoWindow's close window button, or programmatically by the developer, like calling the InfoWindow's show() or hide() functions.