2. Next, we create the buffers that will represent the proximity areas around lakes, schools, and roads. Use **Fixed distance buffer** from the **Processing Toolbox** option to create the following buffers:

+   1. For the lakes, select **Distance** of 500 meters and set **Dissolve result** by checking the box as shown in the following screenshot. By dissolving the result, we can make sure that the overlapping buffer areas will be combined into one polygon. Otherwise, each buffer will remain as a separate feature in the resulting layer:

![](bbox=[0.338, 0.287, 0.662, 0.603])

> It's your choice whether you want to save the buffer results permanently by specifying an output file, or you just want to work with temporary files by leaving the Buffer output file field empty.

+   2. To create the elementary school buffers, first select only the schools with "GLEVEL" = 'E' using the **Select by Expression** tool like we did for the lakes buffer. Then, use the buffer tool like we just did for the lakes buffer.

+   3. Repeat the process for the high schools using "GLEVEL" = 'H' and a buffer distance of 2,000 meters.

+   4. Finally, for the roads, create a buffer with a distance of 1,000 meters.