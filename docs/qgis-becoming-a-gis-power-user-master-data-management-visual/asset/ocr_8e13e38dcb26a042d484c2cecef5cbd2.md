# Setting up and starting TileStream

You must add your MBTiles file under `/home/vagrant/Documents/MapBox/tiles`. You can use your preferred file transfer client to do this; I like WinSCP. Just use the same SSH connection info you used for SSH.

Finally, start TileStream:

```
./index.js
```

Now, you can preview your TileStream service at http://localhost:8088.

Click on the info button to obtain the address to the PNG tiles. You can modify this by removing the reference to the x, y, and z tiles and adding a .json extension to get TileJSON such as http://localhost:8088/v2/c7_8b4e46.json.

Now, simply modify the OpenLayers example to refer to this .json address instead of Mapbox, and you will have a fully nonMapbox use for MBTiles.

The code demonstrating this is at http://localhost:8000/utfgrid-ts.html.

# Summary

In this final chapter, we looked at a web application built on the web services that provides editing capabilities to our user. This is on the simpler end of collaborative geographic data systems but with an attractive cartographic rendering capability offered by TileMill (and Mapnik) and a highly performant data publishing capability through MBTiles and UTFGrid.