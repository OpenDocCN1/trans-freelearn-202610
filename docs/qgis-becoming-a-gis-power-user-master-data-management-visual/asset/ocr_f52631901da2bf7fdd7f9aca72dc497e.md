# Using web services

There are quite a few different types of web-based map service that can be loaded in QGIS. Each type of web service provides data; often, this is the same data in different ways. This recipe is about helping you figure out what type of web service you want to consume, and conversely what type of web service you may want to create for others to use.

# Getting ready

This recipe is all about thinking, so you don't need anything in particular to start. It does help if you have a project in mind and some type of data you are interested in using or creating. Most, if not all, of these methods require an Internet connection or a local server, providing these services. Of course, if you have the data locally, you should probably just load it directly.

To help with the following section, here's a list of acronyms:

+   - **CSW**: This is **Catalog Service for the Web**
+   - **WFS**: This is **Web Feature Service**
+   - **WFS-T**: This is **Web Feature Service, Transactional**
+   - **WCS**: This is **Web Coverage Service**
+   - **WMS**: This is **Web Map Service**
+   - **WMTS**: This is **Web Map Tile Service**
+   - **TMS**: This is **Tile Map Service**
+   - **XYZ**: This is an "X Y Z" Service (there really isn't a formal name for this one because it's not an official standard)

# How to do it...

+   1. Start by answering the following questions with regards to using data over web services:
+      - Do you already know where to find the data that you want?
+      - Do you need to edit or apply custom styling to this data?
+      - Do you care if the data is vector or raster?
+      - Do you need the data at its original resolution or quality?
+      - Do you need data at specific resolutions or in a specific projection?