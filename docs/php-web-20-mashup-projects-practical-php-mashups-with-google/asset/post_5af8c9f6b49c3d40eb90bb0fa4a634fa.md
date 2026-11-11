
## London Tube Photos

```
<?php foreach($linesArr as $lines) { ?>
    <option value="<?= $lines->getLineId() ?>" <?= $_GET['line'] == $lines->getLineId() ? "selected=\"selected\"" : "" ?>><?= $lines->getLineName() ?></option>
<?php } ?>
</select>
<input type="submit" value="Go!" />
</form>
```

This code block draws the form object that we use for line selection. The PHP foreach loop loops through the array of Tube lines to grab each line object.

```
<div id="map" style="width: 800px; height: 600px"></div>

</body>
</html>
```

At this point, the map is functional. You have a mashup that can draw stations in the London Tube system. You can navigate around, select lines, and click on markers to see what stations you clicked on.

![](img/c637937d44400f1da21fb7cca101de80_282_0.png)

## Using Flickr Services with AJAX

With some slight modification, we can add a call to Flickr Services. Generally, the strategy we want is to make an HTTP request with the XMLHttpRequest object when the user clicks on a marker. A good place to do this is in the callback function for the marker's event listener. We already know the name of the station, so we can use it as the basis of a search to Flickr.

This is a very acceptable strategy, but there's a huge problem. In general, browsers cannot make an HTTP request with XMLHttpRequest to another server. This is done to prevent cross-site scripting attacks, in which a malicious website runs code to steal information about sensitive information between a user and another website. In practice, this means that XMLHttpRequest calls can only go back to the server the page originated from. With this limitation, how are we going to use XMLHttpRequest to make a call from our website to Flickr Services?

## Creating an XMLHttpRequest Proxy

The solution is to create a web service proxy on our server. The web server will execute the Flickr Service call, not the browser. Our XMLHttpRequest action will execute a GET request on the proxy and pass Flickr Services parameters to the proxy. The proxy will then make a request to Flickr, and pass the response back, unaltered, to the web browser. The browser doesn't know or care that the true data source is from Flickr. In the examples code, in the services directory, the proxy is named searchFlickr.php. This is a small file whose sole job is to do just that.

```
<?php
require_once('../classes/RESTParser.php');
$restParser = new RESTParser();
```

We will use the REST interface from Flickr. In this code, we will use the same REST parser that we created from Chapter 1 to handle the REST call.

```
$paramArray = array();
foreach ($_GET as $key => $value) {
    if ($key == 'format' || $key == 'nojsoncallback' || $key == 'text')
    {
        $paramArray[$key] = $value;
    } else {
        die("Unallowed Parameter Passed.");
    }
}
```

We initialize an array of Flickr Services parameters. We will pass this array to the REST parser when we actually make the service call. This array is populated by looping through the GET array and adding the array key and values to the $paramArray. As this page is open to the entire world, it is a good idea to put some security around it. Here, we are allowing only three Flickr parameters to be passed from the calling page. Otherwise, the script will die.

```
//Add the API Key to the Request
$paramArray['api_key'] = 'YOUR FLICKR SERVICES KEY';
$paramArray['method'] = 'flickr.photos.search';
$paramArray['per_page'] = '4';
$paramArray['page'] = '1';
$paramArray['text'] .= ' London tube';
```

One additional benefit of this approach is that we get to hide our API key on the server. If it was JavaScript making this call, we would have to expose our key in front end code, and anyone can steal it. This isn't as much of a concern with the Google API Key because that key is restricted by domain. However, there is no such restriction for the Flickr Services key. Here, we add it to the parameter array on the server. The key is passed in server-to-server communication, and the user will never see it.

As an additional security measure, we also specify the Flickr method here. This insures that only the flickr.photos.search method is called from this script.

We are passing two additional parameters to make the results a little more manageable. per_page will limit the results returned from Flickr to just 4 photos per page. We then tell Flickr to return only one page using the page parameter. The result is that a maximum of four photos will be returned by Flickr.

The final line in this block adds "London tube" to our search query terms passed to Flickr. This is solely for the purposes of narrowing the search.

```
echo $restParser->callService($paramArray, 'api.flickr.com', '/services/rest/', 'GET');
?>
```

Finally we pass the array of parameters to the RESTParser's callService() method along with the Flickr Services server and endpoint information. The method returns the response from the server, and we just echo it out to the requester.

## Modifying the Main JavaScript

Now we can modify our mashup's index page. In the examples code for this chapter, there is a file named `index.php`. This is the full, completed home page for the mashup. It is basically `index-Basic.php` from earlier, but with the Flickr Services calls. We will talk about what is different with this version from the basic version.

The first thing we need to do is add a handful more global variables to track.

```
var g_xmlHttp;
var g_stationName;
var g_flickrString;
var g_map;
```

The first, `g_xmlHttp`, is a container for the `XMLHttpRequest` object. The next two, `g_stationName` and `g_flickrString`, are used to hold information from the Flickr Service response. We will talk about the need for them as we encounter them. Finally, `g_map` is the same Google Map container as before.

## Making the XMLHttpRequest

Before we can make `XMLHttpRequest` requests, we need a function to create the `XMLHttpRequest` object when a request is about to be made. This is done through the `createXMLHttpRequest()` function.

```
function createXMLHttpRequest() {
    if (window.XMLHttpRequest) {
        g_xmlHttp = new XMLHttpRequest()
    } else if (window.ActiveXObject) {
        g_xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
}
```

This code uses the standard browser check to see if the browser can create a native `XMLHttpRequest` object or, if it is Internet Explorer, create a `Microsoft.XMLHTTP` request object through `ActiveXObject`. The object is then placed in `g_xmlHttp`.

We call `createXMLHttpRequest()` in the first line of a modified Event Listener callback function.

```
GEvent.addListener(marker, "click", function() {
    createXMLHttpRequest();
    g_stationName = stationName;
    retrieveFlickrPhotos(stationName);
    marker.openInfoWindowHtml("<div style=\"width:220px; height:250px;\">\"<b>\" + stationName + \"</b><p style=\"text-align:center;\"> <img src=\"images/wait.gif\" style=\"padding-top:50px;\" /></p></div>");
});
```

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
