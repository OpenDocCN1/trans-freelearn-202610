
## What Just Happened?

We just transformed a point in the EPSG:4326 projection to a point in the EPSG:900913 projection. Let's take a closer look at the transform method we called on the point_to_transform object.

```
point_to_transform.transform(proj_4326, proj_900913);
```

This will transform the original point from the proj_4326 projection to the proj_900913 projection. Notice, we aren't calling the transform() function of a projection object, but of a LonLat object. The transform() function's definition for an OpenLayers.LonLat object is as follows:

| Function | Description | Parameters |
|---|---|---|
| `transform(source, dest)` | This function transforms a point in place, meaning that the original point will be transformed; hence the original value will be lost. If you need a copy of it, use the `.clone()` method first. It returns itself. | `source`: Source projection<br>`dest`: Destination projection. |

In this case, our source projection is in EPSG:4326, and our destination projection is in EPSG:900913. Keep in mind however, that EPSG:4326 and EPSG:900913 are the only two projections you can do transforms on with OpenLayers if you do not include the Proj4js library.

> When creating your map, all your raster layers (image-based layers; nearly every layer except the vector and image layer) must be in the same projection as your map. You can do projection transformations with coordinates and the vector layer, but once OpenLayers gets back an image from a map server it cannot reproject the image itself (that's something the map server has to do).

## The Proj4js library

The Proj4js library allows you to transform the coordinates from one coordinate system into another coordinate system. The Proj4js website is located at http://proj4js.org. By just including the Proj4js library on your page (like you do with OpenLayers), you can do more transforms within OpenLayers. Not all EPSG codes are supported, but there are many and if your desired code is not supported you can add a projection definition for it.

> The site http://spatialreference.org contains Proj4js definitions for most of the EPSG codes.

Ideally, you should be using the same projection throughout your map, but there are times when you may want to display the coordinates in a different projection—such as with a vector layer. Let's take a look at how to setup the Proj4js library.

## Time for Action – setting up Proj4js.org

This step is similar to the way we set up OpenLayers.

- 1. Download Proj4js from http://trac.osgeo.org/proj4js/wiki/Download. At the time of writing, the latest version was proj4js-1.0.1.zip, so go ahead and download it (or whichever the latest version is).
- 2. Extract it and copy the proj4js folder to your root code directory (the folder where your OpenLayers.js file is located).
- 3. Add the following line in the <head> section of your code after the OpenLayers library inclusion code.
- 4. Now, open up the page and start Firebug. Type and run the following:
- 5. You should see output that looks like the following:

```
<script type='text/javascript' src="proj4js/lib/proj4js-combined.js"></script>
```

```
var test_proj = new Proj4js.Proj('EPSG:4325');
console.log(test_proj);
```

```
Object { srsCodeInput="EPSG:4325", more...}
```

## What Just Happened?

We just included the Proj4js library and tested to see if it worked. If you received an error when you attempted to call new Proj4js.Proj() it means that the location of the proj4js-combined.js file was wrong. Ensure that the directories are set up properly (your example file should be in the same directory as the proj4js folder, and inside the proj4js folder should be a bunch of files and folders, including a lib folder that contains the proj4js-combined.js file).

## Defining custom projections

Now that the Proj4js library is included, you can do transforms with more projections the same way we did in the previous example. Not all projections are defined; however, you are able to define them yourself, for example:

```
Proj4js.defs["EPSG:27563"] = "+title=LAMB sud france +proj=lcc
+lat_1=44.1 +lat_0=44.1 +lon_0=0 +k_0=0.999877499 +x_0=600000
+y_0=200000 +a=6378249.2 +b=6356515 +towgs84=-168,-60,320,0,0,0,0
+pm=paris +units=m";
```

After that, you'd be able to use EPSG:27563 for projection transformations just like you were able to use EPSG:4326 and EPSG:900913 from the earlier examples.

There are a number of already defined projections, and you can view them more extensively at http://proj4js.org. A more complete list (containing Proj4js definitions for nearly any EPSG code) can be found at http://spatialreference.org/.

## Summary

In this chapter, we talked about projections. We covered what they are and the various different types of projections. Longitude, latitude, and other geographic concepts were also discussed. While we just scratched the surface of these pretty complex topics, you should have enough fundamental information to understand how to use projections.

We also talked about the Projection class, along with how to transform coordinates and use the Proj4js library. You'll often work with data in coordinate systems other than EPSG:4326, and knowing how to work with and transform data in other projections is important.

In the next chapter, we'll take a look at Google Maps and other third party APIs, and put some of our recently gained knowledge of projections to use.

## 5

## Interacting with Third Party APIs

*Web maps are very popular today, and are growing in popularity. After Google Maps was introduced, there was an explosion of interactive web maps. Google provides an API to interface with its mapping service, as do others now, and OpenLayers work well with most of them. Not only can we use these third party APIs with OpenLayers, we can also 'mashup' other layers on top of them.*

In this chapter, we're going to learn:

- Concept of third party mapping APIs
- Using the layer classes for third party APIs: Google, Microsoft, and Yahoo!
- How to use OpenStreetMap
- Working with Spherical Mercator
- Combining different layer classes

## Third party mapping APIs

Web based maps are commonplace today. The catalyst for the explosive growth of web maps was the introduction of Google Maps. Web maps existed before, but they were not quick or developer friendly. In June 2005, Google released an API for Google Maps which provided a front end client (the role OpenLayers plays) along with an access to the backend map server.

This allowed anyone to insert not just a Google Map on their site, but also allowed them to add in their own point data and manipulate the map in other ways. Google Maps grew in popularity, and other companies, such as Microsoft and Yahoo!, followed in their footsteps, creating their own web-mapping APIs.

## Map mashups

The term **mashup** refers to an application that combines various different data sources and functionality together. A **map mashup** is a map that combines different layers and data. Third party mapping APIs, like Google and Yahoo! Maps, allow people to more easily create these 'map mashups'. For example, a map with a Google Maps base layer overlaid with markers that track places you've traveled to could be considered a map mashup.

OpenLayers did not introduce map mashups, but it allows us to create very powerful ones with ease. Combining a Google's Map layer, a WMS layer, and a Vector layer is pretty simple with OpenLayers.

## OpenLayers and third party APIs

OpenLayers allows you to use third party mapping APIs inside your map application, letting you use their maps inside of yours. The main caveat is that, at the time of writing, the third party API map must be a base layer (but, as we discussed before, you can use as many base layers as you wish).

The three large commercial mapping APIs that OpenLayers can communicate with are **Google Maps**, **Yahoo! Maps**, and **Bing (Microsoft) Maps**. There is another free and open source API that OpenLayers works well with (and works in a similar manner to the previous three) called **OpenStreetMap**.

We'll first take a look at each of these four Layer classes, and then we'll go over how to combine them with other Layer classes.

## Google Maps

The software behind Google Maps consists of a client and server. The client is what you use when you visit `http://maps.google.com`, and it communicates with the Google Maps backend servers. Google provides an API that lets you use their own client and backend server, but since OpenLayers is used as our client, we're only interested in interacting with Google's backend map server.

> There may be legal restrictions depending on how you plan to use the Google Maps. Full restrictions can be found at `http://code.google.com/apis/maps/terms.html` and `http://www.google.com/intl/en_ALL/help/legalnotices_maps.html`.
