
## Layer.TileCache

The TileCache layer allows you to interact with MetaCarta's TileCache, which is free open source software that caches WMS requests. TileCache provides enormous performance boosts because it caches requests to a WMS server. This means that once a request has been made, the response (the map image) is stored either on the server's hard drive or in memory and is immediately returned to the client.

The normal process of requesting a map image from a WMS server goes like this:

- 1. Client sends request to WMS server, asking for a specific part of the map.
- 2. WMS Server receives the request and generates an image based on the requested map extent.
- 3. WMS Server returns the rendered map image to the client.

The bottleneck is Step 2, the WMS server rendering the image based on the request. With caching (either through TileCache or by other means), the second step becomes a lot easier. With caching, the server only has to generate an image once, and then it saves it (in the hard drive or memory). So, instead of having to generate an image for every request, it simply grabs an image which it has already generated. This makes the request seem almost instantaneous, and most popular web maps use some form of caching—including Google Maps.

We will be discussing TileCache a little more in the last chapter, where we talk about deploying and using OpenLayers in a production environment. The documentation for this layer class is at http://dev.openlayers.org/docs/files/OpenLayers/Layer/TileCache-js.html. For more information on TileCache, head to http://tilecache.org/.

## Layer.Vector

The vector layer is one of the more powerful features of OpenLayers. It allows us to draw points and polygons on the map, style them however we want, retrieve and use KML and other geo data formats, etc. The vector layer makes use of Protocols (such as HTTP), Formats (such as KML), and Strategies (such as Clustering) to display and provide an interactive layer. The vector layer also allows us to create and edit vector features. In Chapter 9, our chapter on the Vector Layer class, we'll go into far more detail.

## Layer.VirtualEarth

This layer allows us to interact with Microsoft's VirtualEarth API. It works in a similar way to the Google Layer, so we won't spend more time discussing it here. Chapter 6 will fully cover this layer class.

## Layer.WFS

This is a class that is deprecated, so try to avoid using it, as it will soon no longer be supported. I wanted to explicitly mention it because it is on its way out, but I still see lots of code that uses it. WFS (Web Feature Service) allows us to interact with data that is stored on the server. If you would like to use WFS, the general procedure is to use the `Layer.Vector` class and use WFS as the format. We'll be covering this in detail in Chapter 9, our chapter on the Vector Layer class.

## Layer.WMS

This is the Layer class we've been using throughout the book so far. It inherits from `Layer.Grid`, and is similar to other classes we've mentioned. WMS is a popular standard and the way we interact with it is similar to how we interact with other Layer classes. We will be using this class throughout the book, so you haven't seen the last of it. Layer class documentation can be found at http://dev.openlayers.org/docs/files/OpenLayers/Layer/WMS-js.html. More information and specifications of the WMS protocol can be found at http://www.opengeospatial.org/standards/wms.

## Layer.Yahoo

This Layer class allows us to interact with the Yahoo! API. It works in a similar way to Layer.Google and Layer.VirtualEarth, and we'll be covering it in more detail in Chapter 6. Documentation for this layer class is at http://dev.openlayers.org/docs/files/OpenLayers/Layer/Yahoo-js.html. The Yahoo! Maps API can be found at http://developer.yahoo.com/maps/.

## Accessing layer objects

The last topic to cover concerns the functions of the base Layer class. Before we do that though, let's go over how to access layer objects in Firebug. We'll be doing this often throughout the book, so it is quite important.

## Time for Action – accessing map.layers

Let's jump in to using some of the Layer class functions.

- 1. Open up the second WMS example, **chapter3_ex2_options_config.html** in Firefox. We won't be editing the code yet.
- 2. Enable Firebug and go to the Console panel.
- 3. In the console, input and run this line of code:
- `map.layers;`
- 4. You should then see a list of layers.

![](img/1b8cd151c9f278555f9cf255b20d244e_99_0.png)

### What Just Happened?

You just accessed the `layers` property of the global `map` object. This is a property of the map object that contains an array of all the layers in the map. The list is ordered by the way you add layers to the map, so the first item in the list corresponds to the first layer we added to the map, the `wms_layer_map` layer. Not only will Firebug list the layer objects, you can also access any one of them by clicking on them.

## Time for Action – accessing layer objects in Firebug

Let's take a look at how to access an object in Firebug now.

Click on one of the items in the array of objects that was outputted from the previous Time for Action section. You should see something like this:

![](img/1b8cd151c9f278555f9cf255b20d244e_100_0.png)

### What Just Happened?

We just accessed our map's wms_layer_map layer object. What you see in the DOM panel is a listing of all the properties and functions that layer object contains. The DOM panel is one way we can access a layer object's properties. The DOM panel is a great way to get a quick look at all the properties and values of any object we wish to know more about. You'll notice some of the properties have not yet been mentioned; this is because some properties are specific to the Layer.WMS class, and is not included in the base Layer class.

## Accessing layer properties

There are two common ways to access a layer object's properties.

### map.layers

The first way is by accessing the layer object through the map.layers array, which we did in the previous example. Here, we are accessing the global map object, which was defined outside the init() function with var map;

If we know the index of a layer object we wish to view, we can use map.layers[n] to access the layer object (where n is the index). For example, if we wanted to access the wms_layer_map layer object, we know it is the first layer added to the map so its index is 0 (because array starts its index at 0), so we would type map.layers[0]. The next layer, wms_layer_labels, has an index of 1, and so on.

Accessing layers this way, however, is not advised. Adding a layer will change the array, as will deleting a layer. The best way to access layer objects is to store a reference to them, like we've been doing in all the examples (e.g., var my_layer = new OpenLayers.Layer(), as we'll go over now.

### Storing references to layer objects

The second way is to define our layer objects as global variables, like how we did with our map object. So far, all our layer objects are defined inside the init() function, which means we can't access them or refer to them outside of the init() function. If we try to access the wms_base_ground layer object in Firebug, we will get an error because the wms_layer_map layer object is not a global variable as we are not 'inside' of the init() function in Firebug. This concept is referred to as scope.

![](img/1b8cd151c9f278555f9cf255b20d244e_101_0.png)

Let's look at our code to see how the map object is defined as a global variable.

```
var map;

function init() {
    //Setup our map object
    map = new OpenLayers.Map('map_element', {});

    // Setup our two layer objects
    var wms_layer_map = new OpenLayers.Layer.WMS( ...
```
```

When you specify var, you are basically saying 'create a new variable here'. Notice how we define var map; outside the init() function. Inside the init() function, we do not use var when we set map equal to the map object, because the map variable has already been created.
