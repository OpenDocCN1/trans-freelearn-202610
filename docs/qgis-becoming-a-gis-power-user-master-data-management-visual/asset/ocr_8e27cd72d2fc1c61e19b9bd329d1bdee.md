# 6

# Extending QGIS with Python

This chapter is an introduction to scripting QGIS with Python. Of course, a full-blown Python tutorial would be out of scope for this book. The examples here therefore assume a minimum proficiency of working with Python. Python is a very accessible programming language even if you are just getting started, and it has gained a lot of popularity in both the open source and proprietary GIS world, for example, ESRI's ArcPy or PyQGIS. QGIS currently supports Python 2.7, but there are plans to support Python 3 in the upcoming QGIS 3.x series. We will start with an introduction to actions and then move on to the QGIS Python Console, before we go into more advanced development of custom tools for the Processing Toolbox and an explanation of how to create our own plugins.

# Adding functionality using actions

Actions are a convenient way of adding custom functionality to QGIS. Actions are created for specific layers, for example, our populated places dataset, popp.shp. Therefore, to create actions, we go to Layer Properties | Actions. There are different types of actions, such as the following:

+   - **Generic actions** start external processes; for example, you run command-line applications such as ogr2ogr

> ogr2ogr is a command-line tool that can be used to convert file formats and, at the same time, perform operations such as spatial or attribute selections and reprojecting.

+   - **Python actions** execute Python scripts