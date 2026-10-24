# Preparing LiDAR data

LiDAR data is becoming more available, and it represents a fundamental source of detailed elevation data. This chapter will show you how to work with LiDAR data in QGIS.

# Getting ready

We will use the Processing framework, so you should be familiar with it.

We will also use LAStools, which is not included with QGIS. Download LAStools binaries from http://lastools.org/download/LAStools.zip and install them on your computer.

Processing has to be configured so that it can find and execute LAStools. Open the Processing configuration by going to the Processing | Options menu and move to the Tools for LiDAR data section, as shown in the following screenshot:

![](bbox=[0.236, 0.394, 0.764, 0.7])

In the LAStools folder field, type the path to the folder where you have installed the LAStools executables.