| Function | Description/Action | Parameters | Return Value |
|---|---|---|---|
| `setOpacity(\nopacity )` | Sets the opacity of a layer based on the value passed in. The passed in `opacity` value is a float with valid values from 0 to 1, with 0 being completely transparent and 1 being fully opaque. 0.5 would be 50 percent transparent. | `opacity:`\n`{Float}` Value of desired opacity. | - |

# Have a Go Hero – call some functions

Now that we've gone over some of the Layer class functions that we'll be making use of throughout this book, try calling them yourself. Open up Firefox, enable Firebug, and try calling some of the above functions on the layer objects (accessing the layer objects through either the `map.layers` array or the global layer objects themselves). Again, don't be afraid to experiment or break things!

# Summary

In this chapter we talked about the Layer class. We covered the concept of layers and the differences between base layers and overlay layers. We went in depth with the WMS Layer class. We discussed the Layer class properties and methods and were introduced to various layer subclasses.

Whew! This was all likely a lot to take in. We covered a lot of Layer classes individually, but we have not yet made a map with different types of Layer classes. This is one of the things that OpenLayers excels at, and is what drives map 'mash ups.' To understand how to go about putting different layer classes together though, we'll need to have a basic understanding of projections—the topic of our next chapter.