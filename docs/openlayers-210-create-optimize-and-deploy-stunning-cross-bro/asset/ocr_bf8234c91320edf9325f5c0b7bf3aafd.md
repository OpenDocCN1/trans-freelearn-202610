# Geometry class

The **Geometry** class is, more or less, the foundation of the feature objects we've seen so far. Specifically, up to this point, we have been using the `Feature.Vector` class as the base class for our feature objects. We'll talk more about that shortly, but for now it is just important to know that the `Feature.Vector` class uses the `Geometry` class to store geometry information about a feature.

But what exactly is the **Geometry** class? In a nutshell, it stores geographic information. Remember the earlier example in this chapter where we added a feature. The code was:

```
var feature_point = new OpenLayers.Feature.Vector(
    new OpenLayers.Geometry.Point(-72, 42)
);
```

We passed in Geometry Point object (we'll get to what the subclasses are in a minute). Now, if executed and looked at it on the map, we'd see a point at the coordinate -72, 42. Then, if we get the feature information via something like `map.layers[1].features[0]`, we'd see information about that feature. When we look at it, we see a geometry property.

Expanding it, or using `map.layers[1].features[0].geometry`, we find out that it has a `bounds` property, along with `x`, `y`, and `ID` properties. This is how feature store geographic information in a geometry object.

Let's take a look again at the code that instantiates a geometry object: `new OpenLayers.Geometry.Point(-72, 42)`. As you might have guessed, we're using the Point subclass of the Geometry class.

# Geometry subclasses—Theory

When working with the Geometry class, we actually almost always use some subclass of it. What do we mean? Think about the Layer classes we've used throughout the book—we've been using subclasses of the base Layer class the entire time (`Layer.WMS`, `Layer.Image`, etc. are all subclasses of the `OpenLayers.Layer` class).

Just like how we work with layers, we almost always work with `OpenLayers.Geometry` *subclasses* when we want a Geometry object. So far, we've only been explicitly calling the `Geometry.Point` subclass, which accepts a single X,Y coordinate and creates a geometry based on the passed in coordinate. There are many more subclasses that let us do things a bit more tricky than just working with points.

Before we cover the subclasses, let's quickly go over some of the methods available to *all* of the subclasses via the base `Geometry` class. All these methods are available to any Geometry subclass, as all the subclasses inherit from the Geometry class.