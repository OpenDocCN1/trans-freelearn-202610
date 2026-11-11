When the AJAX response is returned, the browser will have to update the InfoWindow. At that point, the browser will not know the name of the station that was clicked, so we store it in a global variable. This second statement is more for user friendliness than application functionality.

Whenever someone clicks on the marker, the application should make the AJAX request. Therefore, it needs to be included here in the click event. We will have to define the request execution in a new function, retrieveFlickrPhotos(). This function will actually create the Flickr search parameters, so we need to pass the name of the station to use as the search term.

```
function retrieveFlickrPhotos(stationName) {
    var url = "services/searchFlickr.php? format=
        json&nojsoncallback=1&text=" + escape(stationName);
    g_xmlHttp.onreadystatechange = parseFlickrSearch;
    g_xmlHttp.open("GET", url, true);
    g_xmlHttp.send(null);
}
```

This function first prepares the URL back to our searchFlickr.php service on our server. We add the Flickr Services parameters to this URL. The parameters we pass are summarized as follows:

| Parameter | Value | Notes |
|---|---|---|
| format | json | We want the response to be in JSON. |
| nojsoncallback | 1 | We do not want to automatically execute a callback when the JSON response is received. This functionality will be handled by the XMLHttpRequest callback. |
| text | The station name and extra search parameters | We need to pass the name through the JavaScript escape() function to make it URL-friendly. We also pass the terms "London" and "tube" to narrow our search. The latter is purely to refine our results. |

After that, the call to the service is executed. We define a callback named parseFlickrSearch() to handle the response.