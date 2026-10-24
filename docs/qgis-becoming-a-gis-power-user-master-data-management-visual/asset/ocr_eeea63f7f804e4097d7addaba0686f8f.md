4. You can test and see whether the tiles were created by looking under the directory where you created them. They will be under the directory in the numbered subdirectories given by their Z, X, and Y grid positions in the tiling scheme. For example, here's a tile at `15/9489/12442.png`. That's 15 zoom, 9489 longitude in the grid scheme, and 12442 latitude in the grid scheme.

You will now see the following output:

![](bbox=[0.329, 0.267, 0.671, 0.531])

# Create a layer description file for the TileLayer plugin

Create a layer description file with a `tsv` (tab delimited) extension in the UTF-8 encoding. This is a universal text encoding that is widely used on the Web and is sometimes needed for compatibility.

> Note that the last six parameters are optional and may prevent missing tiles. In the following example, I will use only the z parameters, zmin and zmax, related to map zoom level.

+   1. Add text in the following form, containing all tile parameters, to a new file:
+   
+      `title credit url yOriginTop [zmin zmax xmin ymin xmax ymax]`
+   
+      - For example, `mytiles me file:///c:/packt/c1/data/output/tiles/mytiles/{z}/{x}/{y}.png 1 14 16.`
+      - In the preceding example, the description file refers to a local Windows file system path, where the tiled `.png` images are stored.