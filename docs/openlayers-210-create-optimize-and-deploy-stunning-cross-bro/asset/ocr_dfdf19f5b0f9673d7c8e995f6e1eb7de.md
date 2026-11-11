# Brief overview of the three classes

These classes work together to help us define how the Vector layer behaves. We'll cover each class in more detail soon, but first let's take an overview of them.

# Protocol class

The **Protocol** class controls how the Vector layer communicates with a source of data. Like the Geometry class, we actually use subclasses of the Protocol class. The two common subclasses used are `Protocol.HTTP` and `Protocol.WFS`. The HTTP protocol class allows us to communicate directly with a source that contains vector data, and the WFS protocol class lets us talk to a WFS service.

# Format class

The **Format** class lets us use a type of file (such as a KML file), and turn the data in that file to actual feature objects in our map. We can also take existing feature objects and convert them to a KML, GeoJSON, etc. file format. The vision the OpenLayers developers have is to support more Formats and Protocols for the Vector class, as opposed to having specific subclasses like WFS (which is deprecated). Other layer classes, like the WMS or Google Maps layer, do not use the Format class—this is because they are raster layers, and we do not need to worry about data formats.

More technically, the **Format** class handles the serialization and de-serialization of the feature data. Serialization is a term programmers use which basically means to extract objects from your code and turn them into something like a file, or a human readable string, so those objects can be accessed outside your code. De-serialization does the opposite—it takes a file or string and turns it into an object that your code can use.

So, what does this mean in the context of OpenLayers? Using the format class, we can serialize and de-serialize feature data from a huge variety of different file formats—such as KML, GeoJSON, GeoRSS, ArcXML, etc.

# Strategy class

Lastly, the **Strategy** class is used to control how requests to the server are set up and then what to do with the data returned from the server. The Strategy class is also a base class and we interact with subclasses of it. There are numerous subclasses we can use, such as `Strategy.BBOX` and `Strategy.Cluster`. Unlike the Protocol and Format classes, where we can use only one of the subclasses for each of those classes, with the Strategy class we can use multiple strategies together.

To clarify, let's look at what the `Strategy.BBOX` class does. The Strategy class is used to set up requests and determine what to do with the response. The `Strategy.BBOX` class is one subclass that handles the first part. Using a WFS server, for example, we can use the `Strategy.BBOX` class to send a request to the server that asks only for data within the viewable map extent. So, the BBOX class determines how to set up the request.