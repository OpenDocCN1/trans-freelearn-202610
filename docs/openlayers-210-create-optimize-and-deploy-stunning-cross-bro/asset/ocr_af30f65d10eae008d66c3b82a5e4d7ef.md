4. Let's add a default layer next. If we don't pass a type in, the default value will be used, which is YAHOO_MAP_REG—a street-like map.

```
var yahoo_street = new OpenLayers.Layer.Yahoo(
  "Street",
  {}
);
```

5. Finally we just add the layers to the map:

```
map.addLayers([yahoo_hybrid, yahoo_satellite, yahoo_street]);
```

6. Open the page. You should see something like the following:

![](bbox=[0.28, 0.31, 0.72, 0.61])

# What Just Happened?

We just created a map with a Yahoo! Maps layer. Like the Google Maps layer, we need to include a link to the Yahoo! API—but we do not need an API key. We included the API with:

```
<script src="http://api.maps.yahoo.com/ajaxymap?v=3.0&appid=euzuro-openlayers"></script>
```

Like the Google Maps API script, this script allows OpenLayers to communicate with the Yahoo! Maps API. It also provides us values to use for the type property.