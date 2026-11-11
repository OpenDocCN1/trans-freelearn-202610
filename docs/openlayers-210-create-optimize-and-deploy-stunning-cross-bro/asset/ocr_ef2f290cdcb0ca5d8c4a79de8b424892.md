# Layer Class—Sub and super classes

The `Layer` class is therefore the superclass from which all other Layer classes derive. The `Layer.WMS` layer class is different from the `Layer.Image` class, and each of those two classes has their own unique properties and functions. But both layers also share all the properties of their `Layer` superclass—so all those properties we discussed earlier apply to both classes.

Some Layer classes also inherit from multiple Layer class in a hierarchical way. For example, the `Layer.WMS` class inherits from the `Layer.Grid` class, which inherits from the `Layer` class.

# Other layer types

OpenLayers supports a multitude of different Layer classes. As you may recall from Chapter 1, each Layer class is associated with a different map server back end. The `Layer.WMS` class is used to connect to a WMS map server, and the `Layer.Google` class is used to connect to the Google Maps service.

Because OpenLayers is such an actively developed framework, there are a few layer classes that are deprecated (not recommended to use anymore). Later versions of OpenLayers will deprecate more Layer classes, so we'll try to focus on the classes that should be around for a while. We won't be able to cover every Layer class, but we will discuss the common ones. The Layer classes which we don't mention below will not be covered in this book, either because they are deprecated or outside the scope of this book. Let's take a look.

# Layer.ArcGIS93Rest

This is the class that allows us to interact with ArcGIS Server 9.3 via its REST interface. Interaction with the REST interface is handled by constructing requests with URLs, and is similar in concept to how GET requests work.

The structure to instantiate objects from this class is similar to `Layer.WMS`, as both inherit from the `Layer.Grid` class. ArcGIS Server is a third party, proprietary piece of software from ESRI, but OpenLayers provides excellent support for it. The OpenLayers documentation for the ArcGIS layer can be found at http://dev.openlayers.org/docs/files/OpenLayers/Layer/ArcGIS93Rest-js.html.

> The documentation for ArcGIS Server's REST API can be found at http://resources.esri.com/help/9.3/arcgisserver/apis/rest/index.html.