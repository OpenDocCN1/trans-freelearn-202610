3. Run this again, as shown in the following screenshot, to export the red band:

![](bbox=[0.203, 0.171, 0.796, 0.628])

The Translate algorithm uses the GDAL library underneath. You can also use this library as an independent tool from the console. At the lower part of the algorithm dialog, you will find a text field where you will see the equivalent console call to your current algorithm configuration.

# Handling null values

Null values are a particular type of values that are used to indicate cells where the value for a given layer is not defined. Understanding how to use them is important to avoid wrong results when performing analyses but also to use them as a tool to get better and more correct results. This recipe explains some of the fundamental ideas about null values in raster layers.