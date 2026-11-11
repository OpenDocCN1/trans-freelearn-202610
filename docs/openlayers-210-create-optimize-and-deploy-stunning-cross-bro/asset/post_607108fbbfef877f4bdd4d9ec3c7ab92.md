
```
return false;
};
```

7. Now we will just register the event:

```
map.layers[1].events.register('beforefeatureadded', this, before_feature_added);
```

8. Try to add a feature to the map. The feature should not be added, and your console should log:

```
feature will not be added! Object { feature=Object,  more...}
```

## What Just Happened?

We just showed how to register and unregister an event on our vector layer that gets triggered before a feature is added to the layer. Let's take a look at one more example involving events, and then continue our discussion about how the Vector Layer class works.

## Time For Actions – working with more events

In this example, we'll examine the events related to adding features to the map. We'll look at the feature added event type, which gets triggered when a feature is added to the map (either programmatically or by a user), as well as sketch events which can be triggered by the EditingToolbar control.

- 1. Open up the first example from this chapter. Again, we'll use Firebug.
- 2. This time, we'll demonstrate the featureadded, sketchstarted, and sketchcomplete events. Let's create three appropriate listener functions:
```
function feature_added(feature) { console.log('feature added', feature); };
function sketch_start(feature){ console.log( 'sketch started:', feature); };
function sketch_complete(feature){ console.log( 'sketch done:', feature); };
```
- 3. Now, register the feature added event:
```
map.layers[1].events.register('featureadded', this, feature_added);
```
- 4. Take a look at the map and add a feature. You should see a message after adding a point or after adding a polygon / line (sketch):
```
feature added Object { feature=Object,  more...}
```

5. Now let's register the sketch events. They will get triggered when using any of the EditingToolbar controls.

```
map.layers[1].events.register('sketchstarted', this, sketch_start);
map.layers[1].events.register('sketchcomplete', this, sketch_complete);
```

6. Now, when adding a point or creating a polygon / line, you should see a total of three messages when you finish adding the feature:

```
sketch started Object { feature=Object,  more...}
sketch done Object { feature=Object,  more...}
feature added Object { feature=Object,  more...}
```

## What Just Happened?

We just demonstrated a few more event types. You can also figure out how the events propagate (which events get executed first) by viewing the console log. From the example above, we can see that the sketchcomplete event get executed before the featureadded event.

Even though we registered the featureadded event first, the order that we register the events in does not change the order that the events propagate in. We'll be using more of the vector layer events throughout the chapter, but it shouldn't be much more complicated than the previous two examples. So, let's get on with it!

## Geometry and Feature classes

We've been using feature objects throughout the chapter so far without really talking about the Feature class itself. We also have hinted at the Geometry class, but we haven't gone into any detail so far. This section will cover both classes in a bit more detail so that you can gain a bit more confidence working with them. Don't worry—it's easy, you've already been exposed to both classes.

Before we get into the Feature class though, we should go over the Geometry class, as it's used to create the actual geometry objects that make up a feature object.

## Geometry class

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

## Geometry subclasses—Theory

When working with the Geometry class, we actually almost always use some subclass of it. What do we mean? Think about the Layer classes we've used throughout the book—we've been using subclasses of the base Layer class the entire time (`Layer.WMS`, `Layer.Image`, etc. are all subclasses of the `OpenLayers.Layer` class).

Just like how we work with layers, we almost always work with `OpenLayers.Geometry` *subclasses* when we want a Geometry object. So far, we've only been explicitly calling the `Geometry.Point` subclass, which accepts a single X,Y coordinate and creates a geometry based on the passed in coordinate. There are many more subclasses that let us do things a bit more tricky than just working with points.

Before we cover the subclasses, let's quickly go over some of the methods available to *all* of the subclasses via the base `Geometry` class. All these methods are available to any Geometry subclass, as all the subclasses inherit from the Geometry class.

## Geometry class methods

- **atPoint(lonlat, toleranceLon, toleranceLat)**: This will return a `{Boolean}` indicating whether the geometry object is at the passed in lonlat. The lonlat parameter is an `{OpenLayers.LonLat}` object containing a coordinate to check, and the toleranceLon, toleranceLat parameters specify a `{Float}` number which sets the threshold for the longitude and latitude, respectively. If a point lies within the threshold of the longitude and latitude, `true` will be returned—if not, `false` will be returned. The calculation which determines if the geometry is at the passed in point is an approximation based on the geometry object's bounds.
- **calculateBounds()**: Calling this function will recalculate the bounds of the geometry. It does not return anything. If you wish to see the geometry object's bounds, see `getBounds()`.
- **clearBounds()**: This function will turn the geometry object's bounds object to null.
- **clone()**: This function creates a copy of the geometry and returns a cloned `{OpenLayers.Geometry}` object.
- **destroy()**: Calling this function will destroy the geometry object. Note—if you call this on a geometry object that is already part of a feature object, it will not remove the geometry object from the map.
- **distanceTo(geometry, options)**: Calling this function will calculate the nearest distance between the current geometry object and a geometry object passed in. The options parameter can contain additional calculation options, which vary depending on the geometry subclasses. There is a details option which is a `{Boolean}` and can be specified for all subclasses. If details is set to `true` in the options parameter, this function will return an `{Object}` containing the distance and x0, y0, and y1,y0 values which represent the coordinates of the two geometry objects. If details is not set (or set to `false`), this function will by default return a `{Number}` containing the distance. Take care to mind the units that the map is in.
- **extendBounds(bounds)**: This function will extend the geometry's bounds, including the passed in bounds object, which must be an `{OpenLayers.Bounds}` object. If the geometry does not have a bounds set when this function is called, then it will be set.
- **getArea()**: Returns a `{Float}` containing the area the geometry object covers. This function is redefined in various subclasses, but can be called by all subclasses.
- **getBounds()**: Returns the bounds of the geometry as an `{OpenLayers.Bounds}` object. If no bounds are set, the bounds are calculated. This will return the bounds for a single geometry object—if you want to get the bounds of all the features on your layer, see the Vector class' getDataExtent method.
