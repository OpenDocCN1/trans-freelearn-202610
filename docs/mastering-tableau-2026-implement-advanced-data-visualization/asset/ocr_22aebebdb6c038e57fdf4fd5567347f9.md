+   4. Once connected, you may need to select which layer to display from the available options
+   5. Adjust the map opacity using the Map | Background Layers menu so your data visualizations remain clearly visible on top

> **Note**
> Before we proceed, here's a note on tiling and zooming. Since high-resolution maps may be many gigabytes, it's impractical to require you to download an entire map in order to zoom in on one small area. Tiles solve this problem by enabling multiple zoom levels. A zoom level of 0 results in a single tile (often a 256 x 256-pixel PNG image) that displays the entire map. As the zoom levels increase, the number of map tiles increases exponentially. Also, a proportionally smaller section of the map displays; that is, as the zoom level increases, the area of the entire map that displays decreases and the total number of tiles required to fill the display remains constant. This helps control the amount of data downloaded at any given time.

Next, we'll explore Mapbox.

# Exploring Mapbox

Mapbox provides custom maps and integrates natively with Tableau. To learn how to build your own custom maps, you can check out its website here: https://www.mapbox.com/. Starting with Tableau 2019.3, multiple Mapbox styles are even available by default.

![](bbox=[0.27, 0.48, 0.67, 0.71])

Figure 9.4.2: Background maps

Next to that, Mapbox itself provides an extended mapping service with an accompanying web application that enables users to customize maps. This customizability encompasses fonts, colors, background images, and more. Mapbox provides basic services free of charge, but, of course, more maps and greater bandwidth needs will require an upgrade with an accompanying fee.