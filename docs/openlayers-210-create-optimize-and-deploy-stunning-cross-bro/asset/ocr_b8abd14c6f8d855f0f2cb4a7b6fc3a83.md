◆ `removeControl( control )`: **Parameters**—`control` { `OpenLayers.Control` }. This function takes in a single parameter, an OpenLayers Control object, and removes it from the map. Removing a control will affect the `map.controls` array, so be aware of this if you are using indexes to reference control objects in other places of your code. This is another reason why creating a variable to reference control objects is preferred, as you don't have to worry about the order of the `map.controls` array.

# Time for Action – using control methods

Let's go through a quick example using some of the functions we just covered.

+   1. Open up an existing map (any map is fine).
+   2. In Firebug, input and run the following:
+      ```js
+      var returned_controls = map.getControlsBy('active', true);
+      ```
+   3. Now, if that array is not empty, call the following. If the array is empty, it means that there are no active controls. Try to add some controls to your map until this function returns a non empty array:
+      ```js
+      map.removeControl(returned_controls[0])
+      ```

# What Just Happened?

We just got an array of controls that were active and then removed the first item in the returned array from the map. If the `returned_controls` array is empty, calling `removeControl` will not do anything (you can't remove a `null` control). Let's now look at a larger group of functions.

# Extent/Coordinate/Bounds related

The following functions are all related, in some capacity, to moving or getting position information about the map.

Some of the functions will make use of the **OpenLayers.LonLat** class. This class is used to create a coordinate object, which contains a longitude and latitude value (or x and y values, depending on your projection). To instantiate a **LonLat** object, simply call the class and pass in longitude (x) and latitude (y) values (which are `{Float}` data types):

```
var my_lonlat_object = new OpenLayers.LonLat(-42.18, 42.20);
```