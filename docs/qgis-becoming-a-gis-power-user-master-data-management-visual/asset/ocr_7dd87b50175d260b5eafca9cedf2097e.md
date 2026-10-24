# Creating map tiles

Another popular way to share maps on the Web is **map tiles**. These are basically just collections of images. These image tiles are typically 256 × 256 pixels and are placed side by side in order to create an illusion of a very large, seamless map image. Each tile has a z coordinate that describes its zoom level and x and y coordinates that describe its position within a square grid for that zoom level. On zoom level 0 (z0), the whole world fits in one tile. From there on, each consecutive zoom level is related to the previous one by a power of 4. This means z0 contains 1 tile, z1 contains 4 tiles, and z2 contains 16 tiles, and so on.

In QGIS, we can use the **QTiles** plugin, which has to be installed using the **Plugin Manager**, to create map tiles for our project. Once it is installed, you can go to **Plugins | QTiles** to start it. The following screenshot shows the plugin dialog where we can configure the **Output** location, the **Extent** of the map that we want to export as tiles, as well as the **Zoom** levels we want to create tiles for.

![](bbox=[0.298, 0.4, 0.702, 0.825])