# Joining layer data

We often get data in different formats and information spread over multiple files. Therefore, one important skill to know is how to join attribute data from different layers. Joining data is a way to combine data from multiple tables based on common values, such as IDs or categories.

This exercise shows you how to use the join functionality in **Layer Properties** to join geographic census tract data to tabular population data and how to save the results to a new file.

# Getting ready

To follow this exercise, load the census tracts in `census_wake2000.shp` using **Add Vector Layer** (you can also drag and drop the shapefile from the file browser to QGIS) and population data in `census_wake2000_pop.csv` using **Add Delimited Text Layer**.

![](bbox=[0.21, 0.41, 0.25, 0.45])

> You can also load the `.csv` text file using **Add Vector Layer**, but this will load all data as text columns because the `.csv` file does not come with a `.csvt` file to specify data types. Instead, the **Add Delimited Text Layer** tool will scan the data and determine the most suitable data type for each column.

# How to do it...

To join two layers, there has to be a column with values/IDs that both layers have in common. If we check the attribute tables of the two layers that we just loaded, we will see that both have the `STFID` field in common. So, to join the population data to the census tracts, use the following steps:

+   1. Open the **Layer Properties** option of the `census_wake2000` layer (for example, by double-clicking on the layer name in the **Layers** list) and go to **Joins**.
+   2. To set up a new join action, press the green **+** button in the lower-left corner of the dialog.
+   3. The following screenshot shows the **Add vector join** dialog, which allows you to configure the join by selecting **Join layer**, which you want to use to join the census tracts and the columns containing the common values/IDs (**Join field** and **Target field**):