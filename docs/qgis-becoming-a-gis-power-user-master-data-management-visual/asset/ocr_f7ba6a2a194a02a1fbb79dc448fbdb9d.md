# # Calculating the vulnerability index

GDAL_CALC evaluates an algebraic expression with gridded data as variables. In other words, you can use this GDAL utility to run map algebra or raster calculator type expressions. Here, we will use GDAL_CALC to produce our grid of the vulnerability index values based on the interpolated threshold scores.

Open a Python Console in QGIS (navigate to **Plugins | Python Console**) and copy/paste/run the following code. Again, you may wish to quit Python (using `quit()`) and restart QGIS/Python before running this code, which will produce the intermediate data for our application. This is used to control the unexpected variables and imported modules that are held back in the Python session.

After you've pasted the following lines into the Python interpreter, press *Enter* if it has not been executed. This code, like the previous one, produces a script that includes a range of numbers attached to filenames. It will run a map algebra expression through `gdal_calc` using the respective number in the range. Execute the following:

```
strCmd = ''

for i in range(10, 31):
    j = i - 5
    k = i - 9
    strOgr = 'gdal_calc --A C:/packt/c6/data/prep//temperatureInter{0}.tif -B C:/packt/c6/data/prep/relative_humidityInter{0}.tif -C C:/packt/c6/data/prep/precipitationInter{0}.tif --calc="A+B+C" --type=UInt16 --outfile=C:/packt/c6/data/prep/calc{0}.tiff'.format(i)

    strCmd += strOgr + '\n'

print strCmd
```

Now, run the output from this code in the Windows command console. You can find the output code under `c6/data/output/calculate_index.bat`.

# # Creating regular points

As dynamic web map interfaces are not usually good at querying raster inputs, we will create an intermediate set of locations—points—to use for interaction with a user click event. The **Regular points** tool will create a set of points at a regular distance from each other. The end result is almost like a grid but made up of points. Perform the following steps:

+   1. Add `c6/data/original/delaware_boundary.shp` to your map project if you haven't already done so.