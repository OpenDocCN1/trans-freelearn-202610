
In general, unless you are performing advanced work and understand how Postgres schemas work, place your layers in the **Public** schema. This is the default that everyone expects.

+   4. Click on the following import layer icon:

![](img/2e88bf5938bf48a409c0a534a8775b21_471_0.png)

+   5. A dialog will pop up, providing you with import options.

SQL databases are usually case insensitive, so you can use all lowercase. Also, never use spaces or special characters in table names; this can just lead to headaches later. An occasional underscore is okay.

+   6. Select the layer to import from the drop-down list.
+   7. Fill in a name for the new table.
+   8. Check whether **schema** is set to **public**.
+   9. In most cases, the only thing left to do is check the **Create spatial index** checkbox:

![](img/2e88bf5938bf48a409c0a534a8775b21_471_1.png)

## See also

For more information on PostGIS installation and setup, refer to `http://postgis.net/install`.

For a more in-depth text on using PostGIS, there are many books available, including Packt Publishing's *PostGIS Cookbook*.

## 2
Data Management

In this chapter, we will cover the following recipes:

+   - Joining layer data
+   - Cleaning up the attribute table
+   - Configuring relations
+   - Joining tables in databases
+   - Creating views in SpatiaLite
+   - Creating views in PostGIS
+   - Creating spatial indexes
+   - Georeferencing rasters
+   - Georeferencing vector layers
+   - Creating raster overviews (pyramids)
+   - Building virtual rasters (catalogs)

## Introduction

One of the reasons to use QGIS is its many features that enable management and analysis preparation of spatial data in a visual manner. This chapter focuses on common operations that users need to perform to get data ready for other uses, such as analysis, cartography, or input into other programs.

In this chapter, you will find recipes to manage vector as well as raster data. These recipes cover the handling of data from both file and database sources.

## Joining layer data

We often get data in different formats and information spread over multiple files. Therefore, one important skill to know is how to join attribute data from different layers. Joining data is a way to combine data from multiple tables based on common values, such as IDs or categories.

This exercise shows you how to use the join functionality in **Layer Properties** to join geographic census tract data to tabular population data and how to save the results to a new file.

### Getting ready

To follow this exercise, load the census tracts in `census_wake2000.shp` using **Add Vector Layer** (you can also drag and drop the shapefile from the file browser to QGIS) and population data in `census_wake2000_pop.csv` using **Add Delimited Text Layer**.

![](img/2e88bf5938bf48a409c0a534a8775b21_476_0.png)

> You can also load the `.csv` text file using **Add Vector Layer**, but this will load all data as text columns because the `.csv` file does not come with a `.csvt` file to specify data types. Instead, the **Add Delimited Text Layer** tool will scan the data and determine the most suitable data type for each column.

### How to do it...

To join two layers, there has to be a column with values/IDs that both layers have in common. If we check the attribute tables of the two layers that we just loaded, we will see that both have the `STFID` field in common. So, to join the population data to the census tracts, use the following steps:

+   1. Open the **Layer Properties** option of the `census_wake2000` layer (for example, by double-clicking on the layer name in the **Layers** list) and go to **Joins**.
+   2. To set up a new join action, press the green **+** button in the lower-left corner of the dialog.
+   3. The following screenshot shows the **Add vector join** dialog, which allows you to configure the join by selecting **Join layer**, which you want to use to join the census tracts and the columns containing the common values/IDs (**Join field** and **Target field**):

By default, the names of the new columns are constructed from join layer name with underscore followed by join layer column name. For example, the `STATE` column of `census_wake2000_pop` becomes `census_wake2000_pop_STATE`. You can change this default behavior by enabling the **Custom field name prefix** option, as shown in the previous screenshot. With these settings, the `STATE` column becomes `pop_STATE`, which is considerably shorter and, thus, easier to handle.

### There's more...

The join that you've created now only exists in memory. None of the original files have been altered. However, it's possible to create a new file from the joined layers. To do this, just use **Save as ...** from the **Layer** menu or **Context** menu. You can choose between a variety of data formats, including the ESRI shapefile, Mapinfo MIF, or GML.

Shapefiles are a very common choice as they are still the de facto standard GIS data exchange format, but if you are familiar with GIS data formats, you will have noticed that the names of the joined columns are too long for the 10 character-name length limit of the shapefile format. QGIS ensures that all columns in the exported shapefiles have unique names even after the names have been shortened to only 10 characters. To do this, QGIS adds incrementing numbers to the end of, otherwise, duplicate column names. If you save the join from this example as a shapefile, you will see that the column names are altered to `census_w_1`, `census_w_2`, and so on. Of course, these names are less than optimal to continue working with the data. As described in *How it works...* in this recipe, the names for the joined columns are a combination of joined layer name and column name. Therefore, we can use the following trick if we want to create a shapefile from the join: we can shorten the layer name. Just rename the layer in the layer list. You can even have a completely empty layer name! If you change the joined layer name to an empty string, the joined column names will be `_STATE`, `_COUNTY`, and so on instead of `census_wake2000_pop_STATE` and `census_wake2000_pop_COUNTY`. In any case, it is good practice to document your data and provide a description of the attribute table columns in the metadata.

In any case, it is very likely that you will want to clean up the attribute table of the new dataset, and this is exactly what we are going to do in the next exercise.

## Cleaning up the attribute table

There are many reasons why we need to clean up attribute tables every now and then. These may be because we receive badly structured or named data from external sources, or because data processing, such as the layer joins that we performed in the previous exercise, require some post processing. This recipe shows us how to use attribute table and the **Table Manager** plugin to rename, delete, and reorder columns, as well as how to convert between different data types using **Field Calculator**.

### Getting ready

If you performed the previous recipe, just save the joined layer to a new shapefile; otherwise, load `census_wake2000_pop.shp`. In any case, you will notice that the dataset contains a lot of duplicate information, and the column names could use some love as well. To follow this recipe, you should also install and enable the **Table Manager** plugin by navigating to **Plugins | Manage and Install Plugins**.

### How to do it...

+   1. Our first step to clean up this dataset is to delete duplicated information. From all available columns, we only want to keep `_STATE`, `_COUNTY`, `_TRACT`, `FIPSSTCO`, `TRT2000`, `STFID`, `_POP2000`, `AREA`, and `PERIMETER`.
+   
+   2. To delete the other columns, enable editing using the **Toggle editing mode** button in the upper-left corner of the attribute table or by pressing *Ctrl + E*. This activates the **Delete column** button.
+   
+   3. Alternatively, you can also press *Ctrl + L* to open the **Delete attributes** dialog. This dialog allows us to delete multiple columns at once. Just select all the columns that you want to be deleted, press **OK**, and QGIS will display the reduced attribute table.

![](img/2e88bf5938bf48a409c0a534a8775b21_479_0.png)

> It's worth noting that the changes will only be permanent once you use the **Save edits** button or disable the editing mode and confirm that you want to save the changes.

+   4. Next, we will rename columns to remove the leading underscores in some of the column names. This can be done using the **Table Manager** plugin.
+   
+   5. When you start the plugin (edit mode should be disabled), you will see a list of the layer columns. The plugin allows you to change the order of columns, as well as rename, insert, clone, and delete columns.
+   
+   6. To rename a column, just select it in the list and press the **Rename** button. You'll then be asked to provide a new name. Go ahead and remove the leading underscores from `_STATE`, `_COUNTY`, `_TRACT`, and `_POP2000`.
+   
+   7. Finally, using the **Move up** and **Move down** buttons, you can also rearrange the column order to something more intuitive. We'd suggest moving `STFID` to the first position and `AREA` and `PERIMETER` to the last.
+   
+   8. If you press **Save**, the changes will be saved back to the layer source file. Alternatively, you can also create a new file using **Save as...**.
