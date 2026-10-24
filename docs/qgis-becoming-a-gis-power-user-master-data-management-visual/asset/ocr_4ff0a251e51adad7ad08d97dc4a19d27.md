# How it works...

Given the list of input coordinates and matching output coordinates, a math formula is derived to translate between the two sets. This formula is then applied to all the points in the original data. The result of this is a reprojected dataset from an unknown projection to a known projection.

> The original data is actually EPSG:102719, but we're pretending that we didn't have this piece of information to demonstrate this example.

# There's more...

When picking a reference layer, try to pick something in the projection that you want to use for your maps and analysis. That way you only have to reproject once, as each additional transformation can add an error. There's also more than one way to go about accomplishing this task, including moving the data by hand.

In this particular, example the transformation is autoselected based on the number of GCP point pairs. 4-5 is the first order polynomial, 6-9 is the second order polynomial, and 10+ is the third order polynomial. Refer to the previous recipe in this chapter for more information.

A related topic is *Affine transformations* when you simply want to shift or rotate a vector layer by a known amount. The QgsAffine plugin is great if you already know the parameters, or roughly know how far you want to rotate and shift the vector layer, as it then just needs some math to get the parameters.

> Maybe by the time you read this, all of the difficult things here will be worked in a plugin. Keep an eye open, and try the experimental plugins Vector Bender, vectorgeoref, and Affine Transformations.

# See also

+   - This method is very similar to the *Georeferencing Rasters* recipe and many of the same tips apply to both.
+   - If you want to see how we got the CAD file into an SHP to begin with, look at *Importing DXF/DWG files* in *Chapter 1, Data Input and Output*
+   - See the *Using Rule Based Rendering* recipe in *Chapter 10, Cartography Tips*, for tips on how to visualize the resulting CAD import better by applying attribute based rule filtering