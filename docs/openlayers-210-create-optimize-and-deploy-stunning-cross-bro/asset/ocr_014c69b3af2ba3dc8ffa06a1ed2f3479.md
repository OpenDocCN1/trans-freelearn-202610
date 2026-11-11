# Possible type values

The possible values for the `type` property, at the time of writing, are the following.

| Type | Description |
|------|-------------|
| `VEMapStyle.Shade` | Displays a shaded street/road map, showing elevations. |
| `VEMapStyle.Aerial` | Displays aerial (satellite) imagery. |
| `VEMapStyle.Road` | Displays a road map. |
| `VEMapStyle.Hybrid` | Displays an aerial layer overlaid with a road map. |

Let's cover one more layer type before we start combining different layers together—the **OpenStreetMap** Layer.

# OpenStreetMap

OpenStreetMap (or **OSM**) is a free, wiki-style, map of the world driven by user contributed content. You are able to use your own OSM tiles or ones provided through the OpenStreetMap servers. Unlike the previous third party APIs, there is no `type` property to specify for this layer.

Setting up an OpenStreetMap service and tiles yourself is not too difficult, but it is outside the scope of this book (visit http://wiki.openstreetmap.org/wiki/OpenLayers_Simple_Example for more information on this). Accessing OSM with OpenLayers, however, isn't.

![](bbox=[0.27, 0.55, 0.32, 0.6])

> More information about OpenStreetMap can be found at http://www.openstreetmap.org/.

# Time for Action – creating an OpenStreetMap Layer

+   1. For the OpenStreetMap layer, we do not need to include a script; we can access it outside of the box.
+   2. This will be pretty simple. We just need to create an OSM layer object:

```
var osm_layer = new OpenLayers.Layer.OSM(
    'OpenStreetMap Layer'
);
```