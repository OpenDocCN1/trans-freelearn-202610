# Loading data from databases

QGIS supports PostGIS, SpatiaLite, MSSQL, and Oracle Spatial databases.
We will cover two open source options: SpatiaLite and PostGIS. Both are available cross-platform, just like QGIS.

SpatiaLite is the spatial extension for SQLite databases. SQLite is a self-contained, server-less, zero-configuration, and transactional SQL database engine (www.sqlite.org). This basically means that a SQLite database, and therefore also a SpatiaLite database, doesn't need a server installation and can be copied and exchanged just like any ordinary file.

You can download an example database from www.gaia-gis.it/spatialite-2.3.1/test-2.3.zip (4 MB). Unzip the file; you will be able to connect to it by going to Layer | Add Layer | Add SpatiaLite Layer, using the Add SpatiaLite Layer toolbar button, or by pressing Ctrl + Shift + L. Click on New to select the test-2.3.sqlite database file. QGIS will save all the connections and add them to the drop-down list at the top. After clicking on Connect, you will see a list of layers stored in the database, as shown in this screenshot:

![](bbox=[0.221, 0.449, 0.779, 0.743])

As with files, you can select one or more tables from the list and click on Add to load them into the map. Additionally, you can use Set Filter to only load specific features.