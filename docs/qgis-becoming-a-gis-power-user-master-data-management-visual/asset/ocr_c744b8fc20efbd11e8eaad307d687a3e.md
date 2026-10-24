# What this code does

We will run a Python code to generate a more verbose script that will perform a lengthy workflow process.

+   - For each parameter (factor), it will loop through every day in the range of days. The range will effectively be limited to 06/10/15 through 06/30/15 as the model requires a 10-day retrospective period.
+   - We will run it via ogr2ogr—GDAL's powerful vector data transformation tool—and use the SQLite syntax, selecting the appropriate aggregate value (count, sum, and average) based on the relative period.
+   - It will translate each result by the threshold to scores for our calculation of vulnerability to mildew. In other words, using some (potentially arbitrary) breaks in the data, we will translate the real measurements to smaller integer scores related to our study.
+   - It will interpolate the scores as an integer grid.

# Running a code in Python

Copy and paste the following lines into the Python interpreter. Press *Enter* if the code is pasted without execution. The code also assumes that data can be found in the locations hardcoded in the following (C:/packt/c6/data/prep/ogr.sqlite). You may need to move these files if they are not already in the given locations or change the code. You will also need to modify the following code according to your filesystem; Windows filesystem conventions are used in the following code:

```
```python
# first variable to store commands
strCmds = 'del /F C:\\packt\\c6\\data\\prep\\ogr.* \\n'
# list of factors
factors = ['temperature','relative_humidity','precipitation']
# iterate through each factor, appending commands for each
for factor in factors:
    for i in range(10, 31):
        j = i - 5
        k = i - 9
        if factor == 'temperature':
            # commands use ogr2ogr executable from gdal project
            # you can run help on this from command line for more
            # information on syntax
            strOgr = 'ogr2ogr -f sqlite -sql "SELECT div_field_, GEOMETRY, AVG(o_value) AS o_value FROM (SELECT div_field_, GEOMETRY, MAX(value) AS o_value, date(time_measu) as date_f FROM {2} WHERE date_f BETWEEN date(\\'2013-06-{0:02d}\\') AND date(\\'2013-06-{1:02d}\\') GROUP BY div_field_, date(time_measu)) GROUP BY div_field_" -dialect sqlite -nln ogr -dsco SPATIALITE=yes -lco SPATIAL_INDEX=yes -overwrite C:/packt/c6/data/prep/ogr.sqlite C:/packt/c6/data/prep/temperature.shp \\n'.format(j,i,factor)
```
```