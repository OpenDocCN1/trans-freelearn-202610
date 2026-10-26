# # Working with Rasters

In this chapter, we move on to the realm of spatial data analysis in R. We begin by introducing the properties and usage principles of the classes used to store raster data in R. For that matter, we are going to first introduce the simpler (nonspatial) structures that are conceptually related to rasters: matrices and arrays. We then cover the more sophisticated classes defined in the `raster` package to represent spatial raster data. You will learn to create, subset, and save objects of these classes as well as to query the characteristics of rasters we have at hand. Afterwards, you will learn two basic operations involving rasters: overlay and reclassification. At the same time, we will see some examples of visualizing raster data in R to help us get a better understanding of the data we have.

In this chapter, we'll cover the following topics:

+   - Using matrices to represent two-dimensional sets of numeric values
+   - Using arrays to represent three-dimensional sets of numeric values
+   - Using classes for single band and multiband rasters in the `raster` package
+   - Reading and writing raster files
+   - Exploring the properties of a given raster object
+   - Basic visualization of rasters in R
+   - Subsetting rasters
+   - Converting raster objects to simpler data structures and vice versa
+   - Performing raster algebra operations
+   - Reclassifying raster values