For the purpose of OpenLayers, EPSG codes are referred to as:

```
EPSG:4326
```

The numbers (4326 in this case) after EPSG: refer to the projection identification number. This projection, EPSG:4326, is the default projection which OpenLayers uses. It has been the projection used in all our examples so far, and uses the familiar Longitude/Latitude coordinate system, with coordinates that range from -180° to 180° (longitude) and -90° to 90° (latitude).

# Time for Action – using different projection codes

Let's create a basic map using a different projection.

+   1. Using the template code, recreate your map object the following way. We'll be specifying the `projection` property, along with the `maxExtent`, `maxResolution`, and `units` properties. If we use a projection other than the default projection, we need to tell OpenLayers the type of coordinates to use, and setting the `maxExtent` is one way to do this. The projection we're going to use is `EPSG:900913`, a projection used by Google Maps and other third party APIs.
+   
+   ```javascript
+   map = new OpenLayers.Map('map_element', {
+     projection: 'EPSG:900913',
+     maxExtent: new OpenLayers.Bounds(-20037508, -20037508,
+       20037508, 20037508.34),
+     maxResolution: 156543.0339,
+     units: 'm'
+   });
+   ```
+   
+   2. Save the file, we'll refer to it as `chapter4_ex1.html`.