# What's a layer?

A layer is basically a way to show multiple levels of information independent of each other. Layers are not just a mapping or cartography concept; graphic designers and digital artists make heavy use of layers.

Imagine a printed-out map of a city. Let's say you also have two sheets of transparent paper. One sheet has blue lines that indicate bus routes, and the other sheet contains green lines that indicate bicycle routes. Now, if you placed the transparent sheet of paper with bicycle routes on top of the map, you would see a map of the city with the bicycle routes outlined.

Putting on or taking off these transparent pieces of paper would be equivalent to turning a layer on or off. The order you place the sheets on top of each other also affects what the map will look like—if two lines intersect, you would either see the green line on top or the blue line on top. That's the basic concept of a layer.

# Layers in OpenLayers

OpenLayers is a JavaScript framework, and as discussed earlier is built using Object Oriented Programming. When we want to actually create a layer, we create (or instantiate) an object from an OpenLayers Layer class.

OpenLayers has many different Layer classes, each allowing you to connect to a different type of map server 'back end.' For example, if you wanted to connect to a WMS map server, you would use the `Layer.WMS` class, and if you wanted to use Google Maps you'd use the `Layer.Google` class. Each layer object is independent of other layer objects, so doing things to one layer won't necessarily affect the other.

> How many layers can I have?
> The safest maximum amount of layers you can have on a map at one time depends largely on the user's machine (i.e., their processing power and memory). Too many layers can also overwhelm users; many popular web maps (e.g., Google and Yahoo!) contain just a few layers. If you need to use tons of layers (say, more than fifty), it might be a better idea to create/destroy them as necessary, as having too many layers may slow down your map on some machines.

Whatever the purpose of your web map application is, you will need at least one layer to have a usable map. An OpenLayers map without any layers would be sort of like an atlas without any maps. You need at least one layer—at least one Base layer. All other layers that 'sit above' the base layer are called Overlay layers. These are the two 'types' of layers in OpenLayers.