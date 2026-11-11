This section creates the markers. We use PHP to help us. If a line GET parameter was passed to the page and the array of stations is not empty, then we need to create a marker for each station. Still in PHP, we loop through using a foreach loop. A GLatLng object, represented by point, is created with the PHP object's latitude and longitude properties. If we just use this point and pass it to the map's addOverlay method, we would create a marker on the map. However, we want to do a little extra with it, like create an event.

We use this point and pass it to another function, createMarker(). This function creates a marker, adds an event listener to it, then returns the same marker.

```
// Creates a marker at the given point with the given number label
function createMarker(point, stationName) {
  var marker = new GMarker(point);
  GEvent.addListener(marker, "click", function() {
    marker.openInfoWindowHtml("<div style=\"width:220px;
    height:250px;\"><b>" + stationName + "</b></div>");
  });
  return marker;
}
```

A marker is created in the first line of the function. Remember that the GEvent object is created when you call the Google Map. Its job is to watch for events on all Google Map objects. We tell it to listen for a click on this marker through the addListener() method.

In the callback function parameter, we define what's going to happen when the marker is clicked. Here, we tell the map to open the InfoWindow using openInfoWindowHtml(). We provide HTML as the parameter using the station name. When opened, the InfoWindow will appear over the marker. The name of the station will be the only content in the window.

```
</script>
</head>
<body onload="load() " onunload="GUnload()">
```

In our body tag, we initiate map creation by calling load(). We also add a call to GUnload() when the page is exited. GUnload() is part of the Google Maps API. Its job is to close up any memory leaks. It is always a good idea to call this at an onunload page event whenever you are using Google Maps.

```
<form name="selectionForm" action="index-Basic.php" method="get">
<select name="line">
  <option value="">Select a Line</option>
```