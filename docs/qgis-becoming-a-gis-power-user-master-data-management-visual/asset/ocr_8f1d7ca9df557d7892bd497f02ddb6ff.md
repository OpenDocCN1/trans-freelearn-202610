# Getting ready

NDVI is calculated using a band with red spectral reflectance values, and another one with near-infrared reflectance values. In the sample dataset, you will find two image files named red.tif and nir.tif that can be used to compute NDVI. A project named ndvi.qgs is available, which contains these two layers and a landsat image corresponding to this same area. Open this project.

# How to do it...

1. Open the Processing Toolbox menu and find the algorithm called Vegetation index[slope based]. Double-click on the algorithm item to execute it:

![](bbox=[0.203, 0.352, 0.797, 0.688])

2. Select the red.tif layer in the Red Band field and the nir.tif layer in the Near Infrared Band field. Click on Run to run the algorithm.