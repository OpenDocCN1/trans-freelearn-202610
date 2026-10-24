# Data Input and Output

Functionality provided by the GDAL library, which (mentioned earlier) acts as a provider for raster layers, is also available in the Raster menu. This includes processing and data analysis methods, but it also includes the information tool that is used to describe a raster data source. You will find it by navigating to Raster | Miscellaneous | Info:

![](bbox=[0.29, 0.22, 0.71, 0.52])

# See also

+   - This is a more complex way to retrieve properties as you can call the tool by adjusting the parameters with more details to get additional information. To know more, check the gdalinfo help page at http://www.gdal.org/gdalinfo.html.

# Importing data from text files

Data can be imported from text files, providing some additional about how the geometry information is stored in the text. This recipe shows you how to create a new points layer, based on a text file.