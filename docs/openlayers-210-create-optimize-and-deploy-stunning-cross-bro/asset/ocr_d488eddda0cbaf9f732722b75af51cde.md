2. You should see something like this, with limited zoom levels:

![](bbox=[0.29, 0.18, 0.71, 0.36])

3. Now, let's recreate the map element and set the maxResolution and numZoomLevels properties:

```
map = new OpenLayers.Map('map_element', {
    controls: [
        new OpenLayers.Control.Navigation(),
        new OpenLayers.Control.PanZoomBar(),
        new OpenLayers.Control.LayerSwitcher()
    ],
    maxResolution: 0.3515625,
    numZoomLevels:8
});
```

4. The map should now show zoom levels like:

![](bbox=[0.22, 0.61, 0.78, 0.79])