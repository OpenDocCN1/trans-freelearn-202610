
# See also

- Chapter 7, Raster Analysis I, and Chapter 8, Raster Analysis II, on further raster methods

# Converting vectors to rasters

Occasionally, you want to convert vectors to rasters to facilitate using raster analysis tools such as the raster calculator.

# Getting ready

You'll need a vector layer; this can be a point, line or polygon layer. The best results generally come from polygon layers. We will use `geology_wake2000reclass.shp`. This file is the result of the earlier clipping vectors recipe with a new column added that codes the geology types as integers. For reference, you'll also use `elev_wake_500m.tif` as a matching raster for the area of interest.

# How to do it...

In order to be useful in analysis, here's a checklist:

- 1. Is the vector data in the same projection as the rest of the raster analysis data? If not, reproject it first. Check the following URL for help, `https://docs.qgis.org/2.8/en/docs/training_manual/vector_analysis/reproject_transform.html`.
- 2. Clip the vector data to the analysis extent. You may need to convert a raster into a polygon mask to clip it (refer to the clipping vectors recipe earlier in this chapter).
- 3. Load `geology_wake2000reclass.shp` and `elev_wake_500m.tif`.

> You can only pick numeric fields as raster formats only store a single number per cell. In order to keep the attribute that you want, you may need to use the field calculator to create a new field that reclassifies categories of text into a numeric scheme (for example, 1 = water, 2 = land, and so on) before performing the conversion. If you copy a unique ID as the attribute, there are some tools later that let you rejoin the original attribute table as a value attribute table (refer to the GRASS functions in **Processing Toolbox**).

![](img/2e88bf5938bf48a409c0a534a8775b21_526_0.png)

- 4. Open **Properties** of `elev_wake_500m.tif`:
  1. In the **Metadata** section, scroll down to **Dimensions**. You will want to match either the dimensions or resolution so that your new raster will match the existing elevation data pixels.
  2. Note that **Dimensions** are X: **134**, Y: **124** and **Bands: 1**. The resolution is 499.637,-498.342 pixel size.
  3. Close the dialog.

- 5. Now, open the conversion dialog by navigating to **Raster | Conversion | Rasterize** and follow these steps:
  1. Input the file as `geology_wake2000reclass`.
  2. Name your output `geology_wake.tif` (you will get a warning to set the size or resolution).
  3. Set the raster size: **Width** to **134** and **Height** to **124**.
  4. Click on **OK** to run the process.

The following screenshot shows how the screen will look:

![](img/2e88bf5938bf48a409c0a534a8775b21_527_0.png)

# Common Data Preprocessing Steps

# How it works...

A grid of pixels is created at the specified width, height, and extent. For each cell, an intersection is performed with the underlying vector layer. If more than 50% of the cell intersects with the vector, it's designated attribute is assigned to the cell.

# There's more...

It's really important to match projection and extent before converting to raster. If you fail to do so, then your pixels in different raster layers won't line up perfectly with each other, and either tools won't work or they will introduce a resampling error. If this looks too pixelated (squares) for your liking, consider creating the raster at a higher pixel density.

If you compare the vector version to the new raster, you'll notice that the area in the middle all came out a similar color. This is due to the values used for classification, where the geology that started with the same major component was given the same starting value (for example, PZ all start with 40, and the last number changes based on the letters after PZ).

Looking at the new layer and want to get rid of the black surrounding the real data? This area is no-data, refer to *Chapter 8, Raster Analysis II*.

# See also

- There are other methods to calculate the new value of a pixel to make smoother transitions or intermediate values when multiple polygons are with the same pixel. Refer to the GRASS and SAGA methods in Processing Toolbox for more sophisticated alternatives.

# Building DateTime strings

Date and time data get stored in all sorts of ways. One of the more frustrating issues is that some common GIS formats (Shapefiles) can't store date and time in the same field without making it a string. This is fine for visual display but terrible for use with tools that use DateTime for their functionality, such as the TimeManager Plugin (refer to *Chapter 4, Data Exploration*).

# Getting ready

Use `datetime-example.shp`, which contains a variety of date and time representations to play with.

# How to do it...

- 1. Load `datetime-example.shp`.
- 2. Open the attribute table of `datetime-example`.
- 3. Create a new field. As this is a shapefile, we'll need to use a String of length 30 (or you can use the empty field called `calculated`).
- 4. Turn on layer editing (this is the pencil icon, which is the first icon to the left of the window toolbar).
- 5. From the drop-down list, select **Calculated**.
- 6. Now press the **Calculation** button.

> In older versions of QGIS, you'll need to open the Field calculator, which also works in newer versions but has slightly more steps.

7. In the calculator, we'll use the `substr` String operation (that is, Substring) in combination with the `||` concatenation to rearrange the values from existing fields into a valid `DateTime`:

- 1. The simplest variant is just to combine `shpDate` with `Time` and put a space in between:
  
  `"shpdate" || ' ' || "Time"`

- 2. For a bit more of a challenge, use `Date`:
  
  `substr("Date",7,4) || '-' || substr("Date",1,2) || '-' || substr("Date",4,2) || ' ' || "Time"`

> Note the use of single quotes (') to denote a string as opposed to double quotes (") , which indicate a field name.

8. Experiment with the formulas, checking the results with the calculator preview at the bottom. When satisfied, select OK, and then click on the Update All button:

![](img/2e88bf5938bf48a409c0a534a8775b21_530_0.png)

9. If you're happy with the results, save the edits; if not, toggle editing off and choose Close without saving.

# How it works...

Substr takes three arguments: the field name, the starting position, and the number of characters after this to include. The position index starts at 1.

# There's more...

Using string manipulation, we've combined multiple fields into an ISO standard format for DateTime, which other tools will recognize and be able to utilize.

The fields included in this example data are as follows:

| Field | Type | Explanation |
|---|---|---|
| `Time` | String | This is time in a 24 hour format: hours:minutes:seconds. |
| `datetime` | String | This is a DateTime String from a typical GPS GPX file. |
| `calculated` | String | This is a String that is long enough to hold date and time with padding characters and a timezone UTC offset. |
| `Date` | String | This is a typical date format coming out of a spreadsheet. |
| `shpdate` | Date | This is a date format in the `Date` type within a shapefile, and it can not hold time. |

# See also

- If using database layers, this can all be performed with SQL in SpatiaLite or PostGIS. The ISO date time standard is 8601, and can be found at `http://en.wikipedia.org/wiki/ISO_8601`.

# Geotagging photos

Newer cameras and phones with built-in GPS can be wonderful tools for data collection, as they help keep track of exactly where and when a picture was taken. However, not all cameras have a built-in GPS. You can add geotags afterwards, either with a GPS log from a separate GPS unit or just using a reference map and your memory or notes.

# Getting ready

For this recipe, you'll need a some photos and either a GPS log (`*.gpx`), reference vector, reference raster, or coordinates. We've provided `centerofcalifornia.jpg` in the geotag folder, and the coordinates are in the image itself but also included as a point in `centerofcalifornia.shp`.

You will also need the Geotag photos plugin, which requires the exiftool program to be installed on your system. If exiftool didn't come with your install, you can easily get it from the Web at `http://www.sno.phy.queensu.ca/~phil/exiftool/` or at package repositories (Linux).
