# Viewing Spatial Data

For the examples in this chapter, we will use the sample data provided by the QGIS project, which is available for download from `http://qgis.org/downloads/data/qgis_sample_data.zip` (21 MB). Download and unzip it.

# Loading vector data from files

In this section, we will talk about loading vector data from GIS file formats, such as **shapefiles**, as well as from text files.

We can load vector files by going to **Layer | Add Layer | Add Vector Layer** and also using the **Add Vector Layer** toolbar button. If you like shortcuts, use *Ctrl + Shift + V*. In the **Add vector layer** dialog, which is shown in the following screenshot, we find a drop-down list that allows us to specify the encoding of the input file. This option is important if we are dealing with files that contain special characters, such as German umlauts or letters from alphabets different from the default Latin ones.

![](bbox=[0.212, 0.435, 0.787, 0.653])

What we are most interested in now is the **Browse** button, which opens the file-opening dialog. Note the file type filter drop-down list in the bottom-right corner of the dialog. We can open it to see a list of supported vector file types. This filter is useful to find specific files faster by hiding all the files of a different type, but be aware that the filter settings are stored and will be applied again the next time you open the file opening dialog. This can be a source of confusion if you try to find a different file later and it happens to be hidden by the filter, so remember to check the filter settings if you are having trouble locating a file.