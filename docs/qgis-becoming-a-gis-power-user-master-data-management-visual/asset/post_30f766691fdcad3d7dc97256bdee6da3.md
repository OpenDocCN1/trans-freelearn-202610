
4. Click on the **OK** button to save changes and close the dialog. Now, our data is displayed correctly:

![](img/2e88bf5938bf48a409c0a534a8775b21_742_0.png)

Another option is to clip data to the dateline meridian. This can be done with the `ogr2ogr` tool from the GDAL toolset. To do this, perform the following steps:

- 1. Open the OSGeo4W command prompt if you are a Windows user, or the terminal window if you are a Linux or Mac OS user.
- 2. Change the directory to the folder where `honolulu-flights.shp` is located, for example, the following directory:
  
  `cd c:\data`
- 3. Enter the following command in the command prompt. Note that we first specify the output file and then the input file:
  
  `ogr2ogr -wrapdateline honolulu-flights-wrapped.shp honolulu-flights.shp`

4. After loading the newly-created `honolulu-flights-wrapped.shp` layer into QGIS, you will now see that flights wrapped on the dateline meridian are displayed correctly:

![](img/2e88bf5938bf48a409c0a534a8775b21_743_0.png)

5. Now load the `nz-coastlines-and-islands` layer from the `new-zealand.sqlite` database. You should see two sets of polygons far from each other. This is New Zealand and Chatham islands, which should be located nearby.

![](img/2e88bf5938bf48a409c0a534a8775b21_743_1.png)

To display them correctly, we can use the `ST_Shift_Longitude` function available in the SpatiaLite. To do this, perform the following steps:

- 1. Open the **DB Manager** plugin by clicking on its button on the toolbar or navigating to **Database | DB Manager | DB Manager**.
- 2. Expand the **SpatiaLite** group in the connections tree on the left-hand side of the dialog, and select the `new-zealand` database.
- 3. Open **SQL Window** and run the following query:

```
UPDATE "nz-coastlines-and-islands" SET
Geometry=ST_Shift_Longitude(Geometry)
```

4. Remove the `nz-coastlines-and-islands` layer from QGIS and add it again. Now, New Zealand and Chatham islands will be displayed correctly, as follows:

![](img/2e88bf5938bf48a409c0a534a8775b21_744_0.png)

It is worth mentioning that the `ST_Shift_Longitude` function is also available in PostGIS.

## How it works...

When we enable 'on the fly' CRS transformation, every geometry is reprojected to the given CRS (in our case Pacific centered) so that coordinates now have the same sign.

The `ogr2ogr` tool with the `-wrapdateline` option splits all geometries that cross the dateline and write them to the output file. Geometries that do not cross the dateline are copied without changes.

The `ST_Shift_Longitude` function translates negative longitudes by 360 and as a result, all the data will be in the range of 0-360 degrees and displayed correctly.

## See also

- http://docs.qgis.org/2.2/en/docs/user_manual/working_with_vector/supported_data.html#vector-layers-crossing-180-degrees-longitude

## Working offline

The Internet is an awesome resource, but sometimes you just don't have access to it. For field work, in places with intermittent services, on an airplane, or even in a meeting room, you just might not be able to access all the stuff that you need. By stuff, we're referring to documentation (user and developer), but more importantly database layers (for example, PostGIS) and web service-based layers (for example, WMS, WFS, the OpenLayers plugin, and so on).

This recipe is about caching local copies of the files that you need on your computer before you leave for an unconnected place.

### Getting ready

For this recipe, you will need to open a PostGIS database or WFS and enable the **Offline Editing** plugin that ships with QGIS.

### How to do it...

- 1. Load your layer from PostGIS or WFS.
- 2. Make sure to activate the **Offline Editing** option.
- 3. In the **Database** menu, you should see **Offline Editing**, choose **Convert to offline project**:

![](img/2e88bf5938bf48a409c0a534a8775b21_745_0.png)
- 4. Choose the local file to use to store the data.
- 5. Then, select all of the layers to convert (vector layers only):

![](img/2e88bf5938bf48a409c0a534a8775b21_745_1.png)

- 6. You can now use your PostGIS layers without a network connection to the online database. Go ahead and try to make some edits. (We recommend that you do this on a copy of database or table until you know what you're doing.)
- 7. When you come back to your network, you can now send your changes to the database by choosing the **Synchronize** option:

![](img/2e88bf5938bf48a409c0a534a8775b21_746_0.png)

## How it works...

The basics are straightforward, a copy of you data is saved into a SpatiaLite database locally on your computer. The project file records the change, and you are good to go as SpatiaLite can do anything any other vector data source in QGIS can do.

Be careful when working in a multiuser environment, this does not handle dealing with editing conflicts if multiple users have been modifying the same dataset independently.

## There's more...

Also, there are all sorts of ways to create offline caches of Raster datasets (network files or web services) including `gdalwmscaching` or `mbtiles`. If you plan to need to work away from the Internet for periods of time, plan ahead, and test solutions before actually needing to go offline. No amount of plugins makes up for good planning.

Please remember to check the legality of caching web services (for example, Google and Bing) before doing so. OpenStreetMap is a reliable source of tiles for offline usage without restrictions.

## See also

- GDAL WMS Cache options are available at http://www.gdal.org/frmt_wms.html
- A discussion of mbtiles usage is available at http://blogs.terrorware.com/geoff/2012/11/17/offline-map-tiles-in-qgis/
- A recent plugin to help you set this up is available at https://plugins.qgis.org/plugins/MBTiles2img/

## Using the QspatiaLite plugin

Sometimes, you may not need to load a whole layer into QGIS, but only some subset of it, or perform some calculations on the fly. In such situations, the ability to run complex SQL queries and display their results in QGIS will be very useful.

This recipe shows you how to execute SQL code with the QspatiaLite plugin and load data in QGIS.

### Getting ready

To follow this recipe, you need to create connection to the cookbook.db database created in the Loading vector layers into SpatiaLite recipe in Chapter 1, Data Input and Output. Alternatively, you can use your own SpatiaLite database, but be aware that you might have to alter some of the following SQL statements to match your tables.

Additionally, install the QspatiaLite plugin from Plugin Manager.

### How to do it...

Make sure that you created a connection to cookbook.db. Start the QspatiaLite plugin by navigating to Database | SpatiaLite | QspatiaLite:

![](img/2e88bf5938bf48a409c0a534a8775b21_747_0.png)

To execute the SQL query, perform the following steps:

- 1. Select database you want to use from the combobox in the top-left corner of the plugin dialog.
- 2. In the **SQL** tab, enter following query:

```
SELECT "census_wake2000".'pk' AS id,
"census_wake2000".'geom' AS Geometry,
"census_wake2000".'area', "census_wake2000".'perimeter'
FROM "census_wake2000" WHERE "census_wake2000".'perimeter' > 100000;
```

You can easily insert the table and column names by double-clicking on them in the **Tables** tree on the left-hand side of the dialog.
- 3. Click on the **Run** button at the bottom of the dialog to execute the query. The **Result** tab will open automatically and you can examine the query results in the table representation, as follows:

![](img/2e88bf5938bf48a409c0a534a8775b21_748_0.png)

If the query results contain geometry information (the so-called geometry column), you can display them in QGIS. To do this, perform the following steps:

- 1. Switch back to the **SQL** tab.
- 2. In the **Option** combobox, select the action that you want to perform, for example, **Create Spatial View & Load in QGIS**.
- 3. In the **Table** field, enter name of the resulting view, for example, `above100k`.
