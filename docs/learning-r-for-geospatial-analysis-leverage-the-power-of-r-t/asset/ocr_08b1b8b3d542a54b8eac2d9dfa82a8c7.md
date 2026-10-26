# 5

# Working with Points, Lines, and Polygons

In this chapter, we will cover the basic usage of the second major type of spatial data—vector layers (points, lines, and polygons). In GIS terminology, these data are sometimes referred to as vectors, but we will use the term *vector layers* to distinguish them from vectors in R (see Chapter 2, *Working with Vectors and Time Series*). We will review the architecture of the vector layer classes defined in the sp package. Examples of the most common operations involving vector layers will then be presented using the sp, rgdal, and rgeos packages.

In this chapter, we'll cover the following topics:

+   - Classes for spatial vector layers (points, lines, and polygons) in the sp package
+   - Creating point layers by geocoding
+   - Reading and writing vector layer files
+   - Exploring the properties of vector layers
+   - Accessing and modifying attribute tables
+   - Reprojecting vector layers
+   - Calculating derived geometrical properties (for example, polygon area)
+   - Querying relations (for example, intersection) between a pair of layers
+   - Creating new layers based on a single layer or a pair of layers
+   - Joining data to an attribute table based on the location or common columns