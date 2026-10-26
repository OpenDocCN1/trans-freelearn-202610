# # Spatial Interpolation of
# Point Data

Spatial interpolation is an example of a geostatistical analysis technique with a wide range of applications. In this chapter, we are going to learn how spatial interpolation can be carried out in R through examples of interpolating meteorological point measurements to create annual temperature maps of Spain.

The purpose of this exercise is two-fold. First, we will see how several common interpolation methods are applied in practice in R. We will see, for instance, that specialized classes are used to represent the input data and/or the results of statistical analyses in R, and witness the advantages of such an approach. Second, we will see how, through the use of loops, we can automate complex tasks such as spatial interpolation, and perform them repeatedly in order to accomplish otherwise unfeasible tasks.

> In this chapter, we are going to use objects previously created in *Chapter 3, Working with Tables.*

In this chapter, we'll cover the following topics:

+   - Spatially interpolating point data
+   - Calculating an empirical variogram
+   - Automatically fitting variogram models
+   - Calculating the **root mean square error (RMSE)** of prediction