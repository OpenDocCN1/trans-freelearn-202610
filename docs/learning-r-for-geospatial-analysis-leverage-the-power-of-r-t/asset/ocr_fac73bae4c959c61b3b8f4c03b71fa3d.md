# Combining Vector and Raster Datasets

The graphical output is shown in the following screenshot:

![](bbox=[0.273, 0.176, 0.727, 0.599])

The preceding screenshot shows the continuous NDVI>0.2 patches, which are 1 km² or larger, within the studied area. Two of these, as expected, are the forests we would like to examine. How can we select them? Obviously, we could export pol to a Shapefile and select the features of interest interactively in a GIS software (such as QGIS), then import the result back into R to continue our analysis. The raster package also offers some capabilities for interactive selection (that we do not cover here); for example, a function named `click` can be used to obtain the properties of the pol features we click in a graphical window such as the one shown in the preceding screenshot. However, given the purpose of this book, we will try to write a code to make the selection automatically without further user input.