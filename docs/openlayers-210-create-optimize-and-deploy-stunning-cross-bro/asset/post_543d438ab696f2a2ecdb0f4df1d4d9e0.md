
**Iterative development** is an important, popular, and effective way to develop applications. The core idea is that you create something simple, get it working, and then improve it. You can figure out more quickly what does and doesn't work by improving on and learning from previous iterations.

Another strategy we'll make use of is **modular programming**. What this means, essentially, is that we try to keep things as discrete (or modular) as possible. By doing so, once we know a component works, we don't have to worry about it later.

## Creating a web map application using Flickr
Because OpenLayers is so flexible, it's easy to make third party software and data work with our maps. Sharing geospatial data is becoming more popular, with services such as Flickr and Twitter freely offering geospatially embedded data. Being able to visualize data often helps us to understand it. Using OpenLayers, we can place geospatial data (say, twitter posts or Flickr images) on a map and get a clearer picture about the data.

## Note on APIs
Many popular sites provide an **API (Application Programming Interface)** which allows programmers to interact with their data. For instance, both Flickr and Twitter provide APIs that enable developers to view recent updates (photo uploads and tweets). These APIs (but not all APIs) let us get geospatial data that we can use with OpenLayers. Flickr provides some very easy to use methods to retrieve data with associated geographic information, so we'll focus this chapter on building a web-mapping application around Flickr.

## Accessing the Flickr public data feeds
While Flickr provides a very robust developer API, we'll only interface with Flickr via URL calls which provide access to feeds. Feeds provide information about data, and we can get different kinds of feeds (e.g., a specific user's feed or the feed for all users combined). It's really quite easy to do so; we just make a call to a URL and specify certain parameters. The base URL we'll call is `http://api.flickr.com/services/feeds/geo/?format=kml`.

The `format` parameter can be a number of values—JSON, RSS, SQL, etc. We'll be using KML and JSON in this chapter. When calling this URL, a file in the format you ask for will be returned which contains information about the latest photos uploaded that have geographic information associated with them (that's what the `geo` in the URL is for).

![](img/1b8cd151c9f278555f9cf255b20d244e_331_0.png)

> Flickr's API documentation can be found at http://www.flickr.com/services/api/. More information about the Flickr feeds can be found at http://www.flickr.com/services/feeds/.

## Specifying data
We can refine what data is returned by adding additional parameters to the URL. You can specify a user's ID via the ID key, a group via the g key, and tags via the tags key. You can also specify other things, such as a certain coordinate and radius—for now, we'll just be focusing on the tags key.

## How we'll do it
Let's create a web map application that will pull in data from Flickr and display it on a map. This will allow us to see, geographically, where photos were submitted from. We could, for example, search for 'bird photos' and get an idea where some particular bird species might be common (or at least, commonly photographed and uploaded to Flickr). We'll break down the development of this application into a few different examples, so let's get started.

## Time For Action – getting Flickr data
The first step in our application will require us to get data from Flickr. We'll request some data and then save it to the server.

- 1. The first step is to figure out what sort of data we want to get. We'll use the URL we mentioned before, but we'll also specify a tag. Let's use 'bird' as a tag. Open up this URL in your web browser, and you should be able to download it as a KML file `http://api.flickr.com/services/feeds/geo/?format=kml&tags=bird`.
- 2. Save the file as `flickr_data.kml` and place it in your map directory.
- 3. Open up the file and take a look at it. We won't be editing it, but just take a look to see how the data is structured. Notice that there are `style` tags—if we want, we can directly apply the styles from the file to our map (as we'll see soon).

## What Just Happened?
We just downloaded the latest images in KML format that contained a tag called 'bird.' When you call the URL and pass in some tag, Flickr will return back to you the latest images uploaded that have some geographic information associated with them.

## Have a Go Hero – accessing the Flickr API
Make a request to the URL from the previous example. Try changing the `tags` parameter and notice how the returned file is different. Based on the parameters in the URL, Flickr will return different data.

## Why did we do this?
You may be wondering why we downloaded the KML file. If you remember from Chapter 9, we could just create a vector layer and point to the Flickr URL instead of a local file. However, if we download the KML file we then have direct control over it; we can modify it. When we're in the 'development mode', we want to keep things as easy to debug and fix as possible.

## Reducing possible errors
Once we get the code working to load the KML file we downloaded (in the next example), we don't need to worry about it anymore. When we develop our application and find bugs or errors with it, we don't have to spend much time tracking them down. We get the static file working, and we move on to the next task. We don't have to worry about the Flickr API going down, our requests not completing, or other things—we can focus on other parts. Once we get more parts of our application working, then we'll switch to using the dynamic URL. For now, let's use the static file.

![](img/1b8cd151c9f278555f9cf255b20d244e_332_0.png)

> Because we'll be loading in a file, you must run this code on a server. You can run it on localhost—if you don't have a server, you can download Apache for windows at http://www.apachefriends.org/en/xampp.html.
> The reason it needs to run on a server is because the KML file is loaded via an AJAX call, and the files must reside on the same domain as the originating request. See Chapter 9 for more details on loading files.

## Time for Action – adding data to your map
Time to get back to coding. Now that we have the data from Flickr from the previous example, let's add it to our map. We'll create a Google Maps base layer and then a vector layer filled with the Flickr data on top of it.

- 1. First, we need to make sure the `flickr_data.kml` file is in the same directory that we'll be creating our map in. Save the file in your server's document root folder, which may be something like (with XAMPP on windows) `c:/xampp/htdocs` or on OSX (with XAMPP, http://www.apachefriends.org/en/xampp-macosx.html) in `/Applications/XAMPP`, or on Linux: `/var/www/`. We'll refer to it as `example_1.html`.

- 2. Now let's create the map. We won't be using a WMS layer this time—instead, we'll just be using a Google base map and place our vector layer on top of it. Make sure to first add the reference to the Google Maps API in the `<head>` section:
```
<script src="http://maps.google.com/maps/api/js?sensor=false"></script>
```

- 3. Now, create and add a Google Maps layer.
```
var google_map = new OpenLayers.Layer.Google(
    'Google Layer',
    {}
);
map.addLayer(google_map);
```

- 4. We'll now need a vector layer. We'll have to define its projection as `EPSG:4326` because that's the projection the coordinates in the KML file are in. If they were in a different projection, we would define it as whatever projection they were in (although, we would need Proj4js to do projection transformations).

We'll use the `HTTP` protocol because we'll be accessing the KML file from our local server. This is why we can't simply open up the page in a folder—when we do so, the protocol is `file:///`, so we must open it from our local web server (provided you have one installed, see the previous note).

Inside the protocol, we'll set up a `format` object from the `KML` class and set `extractAttributes` to `true` so we can access the attributes associated with each image. Finally, we'll use a `Fixed` strategy.
```
var vector_layer = new OpenLayers.Layer.Vector('Flickr Data',
    {
        projection: new OpenLayers.Projection('EPSG:4326'),
        protocol: new OpenLayers.Protocol.HTTP({
            url: 'flickr_data.kml',
```
