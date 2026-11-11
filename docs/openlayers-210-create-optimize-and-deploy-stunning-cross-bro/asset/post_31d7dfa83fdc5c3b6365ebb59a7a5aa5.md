
## Brief overview of the three classes

These classes work together to help us define how the Vector layer behaves. We'll cover each class in more detail soon, but first let's take an overview of them.

## Protocol class

The **Protocol** class controls how the Vector layer communicates with a source of data. Like the Geometry class, we actually use subclasses of the Protocol class. The two common subclasses used are `Protocol.HTTP` and `Protocol.WFS`. The HTTP protocol class allows us to communicate directly with a source that contains vector data, and the WFS protocol class lets us talk to a WFS service.

## Format class

The **Format** class lets us use a type of file (such as a KML file), and turn the data in that file to actual feature objects in our map. We can also take existing feature objects and convert them to a KML, GeoJSON, etc. file format. The vision the OpenLayers developers have is to support more Formats and Protocols for the Vector class, as opposed to having specific subclasses like WFS (which is deprecated). Other layer classes, like the WMS or Google Maps layer, do not use the Format class—this is because they are raster layers, and we do not need to worry about data formats.

More technically, the **Format** class handles the serialization and de-serialization of the feature data. Serialization is a term programmers use which basically means to extract objects from your code and turn them into something like a file, or a human readable string, so those objects can be accessed outside your code. De-serialization does the opposite—it takes a file or string and turns it into an object that your code can use.

So, what does this mean in the context of OpenLayers? Using the format class, we can serialize and de-serialize feature data from a huge variety of different file formats—such as KML, GeoJSON, GeoRSS, ArcXML, etc.

## Strategy class

Lastly, the **Strategy** class is used to control how requests to the server are set up and then what to do with the data returned from the server. The Strategy class is also a base class and we interact with subclasses of it. There are numerous subclasses we can use, such as `Strategy.BBOX` and `Strategy.Cluster`. Unlike the Protocol and Format classes, where we can use only one of the subclasses for each of those classes, with the Strategy class we can use multiple strategies together.

To clarify, let's look at what the `Strategy.BBOX` class does. The Strategy class is used to set up requests and determine what to do with the response. The `Strategy.BBOX` class is one subclass that handles the first part. Using a WFS server, for example, we can use the `Strategy.BBOX` class to send a request to the server that asks only for data within the viewable map extent. So, the BBOX class determines how to set up the request.

Another thing the Strategy class does is control what happens to the data returned from the server. Using a class like Strategy.Cluster, when the layer gets back the data it will group the returned data together in clusters. Therefore, it helps to control what happens with the results of the response to the server.

We can use the BBOX class and Cluster classes together—i.e., set up the layer to ask a WFS server for data within the map's extent, then the layer receives the data which it will turn it into clusters.

## How these three classes interact

Now, you might have started to get an idea how these classes interact. The Protocol class determines that the data is communicated to and from the client. That data is in some format which is serialized and de-serialized by the Format class. Finally, the Strategy class tells the Vector class how to set up the request to the server and what to do with the response.

## Example instantiation

Before we dive into examples or further discussion, let's take a look at how we could create a vector layer using all these three classes. The following will load in feature data from a GeoJSON file. The file must be located on the same server—if it's on a different server, or if you wish to point to an external URL, you'll need a proxy host (which we'll cover later).

```
var vector_layer = new OpenLayers.Layer.Vector('More Advanced Vector Layer',{
    protocol: new OpenLayers.Protocol.HTTP({
        url: 'some_data.json',
        format: new OpenLayers.Format.GeoJSON({})
    }),
    strategies: [new OpenLayers.Strategy.Fixed()]
});
```

The above code would use the HTTP Protocol class and use the GeoJSON format. Notice how the format is actually part of the protocol object. The Fixed Strategy class is also used, which essentially just causes the vector layer to request features once and never request new features again.

That's actually pretty much all there is to it! Different formats would use a different Format class, and if you wanted to use other strategies you'd simply pass them into the strategies array.

That's a lot to take in. Let's go through two examples which show how to use these three classes together.

## Time for Action – creating a Vector Layer

In this first example, we'll use the code similar to what we just saw above—making use of all three classes to build our vector layer, which will request data from a URL.

There are two caveats to this example. The first is that it assumes you have a file called ex5_data.json in the same directory as your map file. You can find it in this book's source code files at http://vasir.net/openlayers_book/files/.

Secondly, unless you are running this on a server, it won't work. It will need a proxy host, which we'll cover right after this example. If you do not have access to a server, you can view the example in action at the URL above.

- 1. Start a new map file using the base template from Chapter 1. We'll use the Protocol, Format, and Strategy classes. So first, let's create objects for each of them. Let's start with the format object, as the protocol object will use it. We'll use the GeoJSON format.
  
  ```js
  //Create a Format object
  var vector_format = new OpenLayers.Format.GeoJSON({});
  ```
  
- 2. Now, let's create a protocol object using the format object just created:
  
  ```js
  var vector_protocol = new OpenLayers.Protocol.HTTP({
      url: 'ex5_data.json',
      format: vector_format
  });
  ```
  
- 3. Finally, we'll create a strategy variable to use which will actually be an array of the strategy object.
  
  ```js
  var vector_strategies = [new OpenLayers.Strategy.Fixed()];
  ```
  
- 4. Now, put that together to create the vector layer and add it to the map:
  
  ```js
  //Create a vector layer that contains a Format, Protocol, and Strategy class
  vector_layer = new OpenLayers.Layer.Vector('More Advanced Vector Layer',{
      protocol: vector_protocol,
      strategies: vector_strategies
  });
  
  map.addLayer(vector_layer);
  ```
  
- 5. If you are running this on a server, you can open up the page and you should see something like:

![](img/1b8cd151c9f278555f9cf255b20d244e_287_0.png)

## What Just Happened?

We just demonstrated how to set up a vector layer using **Format**, **Protocol**, and **Strategy** classes. If we wanted to use a, say, KML file, we would just use the `Format.KML` class and change the URL to point to a KML file.

Now, the previous example will not work if you are just running it from a folder—it needs to be on a server. Setting up a server is outside the scope of this book, but let's talk about why the previous example doesn't work if you just open it from a folder.

## Cross server requests

JavaScript uses an `XMLHttpRequest` to make AJAX requests. When using the `Protocol` classes, OpenLayers makes an `XMLHttpRequest` to get the feature data from the URL. If that data file is on a different server than your map, the request will not work. JavaScript has security restrictions in place that prevent it from making these sort of cross domain requests. So, to get around this, we have to use a **proxy**. Other layer classes do not need to do this because other raster layers request images, but do not use AJAX—so the cross domain request restrictions do not apply.
