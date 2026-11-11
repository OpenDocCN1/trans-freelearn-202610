
So, the process for making an object global is to:

- 1. Define the object/variable outside of the `init()` (or any function) with var `variable_name`;
- 2. When you create the object inside the `init()` function, do not specify var when referring to `variable_name`.

Notice our code to create the `wms_layer_map` layer object: `var wms_layer_map = new OpenLayers.Layer.WMS(...)` Since we are using var inside the `init` function, the object is only now being created, and can only be referenced inside the `init` function. The `wms_layer_map` object will only be accessible inside the same scope that it was defined. In this case, since it was defined inside `init()`, we can only refer to it with the code that is inside the `init()` function. To define it globally to access it outside the function, we could do:

```
var map;
var wms_layer_map
function init(){
  map = new OpenLayers.Map ...
  wms_layer_map = new OpenLayers.Layer.WMS( ...
}
```

![](img/1b8cd151c9f278555f9cf255b20d244e_102_0.png)

> Using global variables can make things easier, but try to avoid using them if possible. If you do use them, keep track of them and take care when naming them.

## Pop Quiz – working with Variable Scope

Take a look at the following code. What will be the value of the variable `final`?

```
var a = 3;

//Create a function that will set a variable called 'b' equal to 5
function set_variable(){
  var b = 5;
}

//Call the function
set_variable()
final = a + b;
```

- 1. 8
- 2. 3
- 3. 5
- 4. There will be an error because the variable b cannot be accessed outside the test function.

## Layer class methods

We've covered the properties of the base layer class and discussed most of the layer subclasses so far. The remaining topic to cover is the functions or methods of the base layer class. Since we'll be talking about the functions of the layer class, all layer subclasses inherit and use these functions. Before we get to the functions though, let's cover how to access layer objects in Firebug.

> What's the difference between a function and a method? A method is just a term for a function that is owned by an object or a class.

## Time for Action – defining a global layer object variable

Let's make the layer objects global variables access some properties of a layer, and call one of the layer's methods.

- 1. Make a copy of `chapter3_ex2_options_config.html`. Add the following lines right above the `init()` function:
- 2. Inside your `init()` function, remove the `var` declaration before each of the layer object names. For example, the first line of your `wms_layer_map` definition should now look like this:
- 3. Save the file (we'll refer to it as `chapter3_ex5_global_variables.html`).
- 4. Open it up in Firefox and enable the Firebug console.
- 5. In the console panel, input and run: `wms_layer_map;`. You should see some output like this:
- 6. Input and run the following command in Firebug:

```
var wms_layer_map, wms_state_lines, wms_labels, wms_water_depth, wms_roads;
```

```
wms_layer_map = new OpenLayers.Layer.WMS(
```

```
Object { options=Object, more... }
```

```
wms_layer_map.name
```

- 7. Here we are directly accessing a property of the layer. You should see the layer's name property output:
  
  `Base layer`
  
- 8. Input and run the following command in Firebug:
  
  `wms_layer_map.setVisibility(false);`
  
- 9. Assuming you are at the full extent, you should no longer see the layer now, as you've just turned off the base layer's visibility.

## What Just Happened?

We just made `wms_base_ground` a global variable, accessed a property of it, and called one of its methods. Any of the Layer class properties we talked about before can be accessed via `layer_object.property;`.

So, now that we're a bit more familiar with how to access layer properties and functions on the fly, let's go over the methods of the base layer class. Now, just like properties, most layer subclasses (e.g., `Layer.WMS`) have their own set of functions, but also inherit all the functions from the base layer class.

## Layer class method definitions

All of the following functions can be called the same way we called the `setVisibility()` function; by calling `layer_object.function_name()` ; Some methods require that you pass arguments into them. We won't cover all the functions of the base layer class, as some are either deprecated, found in the Map class function list, or outside the scope of this book.

The required arguments (if any) are listed under the parameters column.

| Function | Description/Action | Parameters | Return Value |
|---|---|---|---|
| `setName( newName )` | Set a new name for the layer based on the passed in string. This will also update the display name for the layer in the layer switcher control, and may trigger a changelayer event. | `newName`: `{String}` The new name of the layer. | - |
| addOptions( options ) | Add additional options to the layer. The parameter behaves like the options parameter, and allows you to set properties on the layer. | options: {Object} An anonymous object containing key:value pairs of layer properties and values. | - |
| redraw() | Redraws the layer. Returns true if the layer was successfully redrawn or false if the layer could not be redrawn. You will need to call this while adding features to a vector layer, as we'll cover in Chapter 9. | - | {Boolean} |
| getVisibility() | Checks if the layer is visible or not. Returns true if the layer is visible, false if not. | - | {Boolean} |
| setVisibility( visibility ) | Sets the visibility to true or false, hide/show the layer, and redraw it. The visibility determines if a layer should be shown or not. | visibility: {Boolean} true to show the layer, false to hide the layer. | - |
| calculateInRange() | Returns true or false if the layer can be displayed at the map's current resolution. If the alwaysInRange property is true, this will always return true. | - | {Boolean} |
| setIsBaseLayer( is_base ) | Will turn the layer into a base layer if true is passed in and if false is passed in it will no longer make it a base layer. | is_base: {Boolean} Set to true to make layer a base layer, false to make it an overlay layer. | - |
| `setOpacity( opacity )` | Sets the opacity of a layer based on the value passed in. The passed in `opacity` value is a float with valid values from 0 to 1, with 0 being completely transparent and 1 being fully opaque. 0.5 would be 50 percent transparent. | `opacity:` `{Float}` Value of desired opacity. | - |

## Have a Go Hero – call some functions

Now that we've gone over some of the Layer class functions that we'll be making use of throughout this book, try calling them yourself. Open up Firefox, enable Firebug, and try calling some of the above functions on the layer objects (accessing the layer objects through either the `map.layers` array or the global layer objects themselves). Again, don't be afraid to experiment or break things!

## Summary

In this chapter we talked about the Layer class. We covered the concept of layers and the differences between base layers and overlay layers. We went in depth with the WMS Layer class. We discussed the Layer class properties and methods and were introduced to various layer subclasses.

Whew! This was all likely a lot to take in. We covered a lot of Layer classes individually, but we have not yet made a map with different types of Layer classes. This is one of the things that OpenLayers excels at, and is what drives map 'mash ups.' To understand how to go about putting different layer classes together though, we'll need to have a basic understanding of projections—the topic of our next chapter.

## 4

## Wrapping Our Heads Around Projections

When you look at a map, you are looking at a two dimensional representation of a three dimensional object (the earth). Because we are, essentially, 'losing' a dimension when we create a map, no map is a perfect representation of the earth. All maps have some distortion.

The distortion depends on what projection (a method of representing the earth's surface on a two dimensional plane) you use. In this chapter, we'll talk more about what projections are, why they're important, and how we use them in OpenLayers. We'll also cover some other fundamental geographic principles that will help make it easier to better understand OpenLayers.

In this chapter, we will cover:

- Concept of map projections
- Types of projections
- Longitude, latitude, and other geographic concepts
- OpenLayers projection class
- Transforming coordinates

Let's get started!
