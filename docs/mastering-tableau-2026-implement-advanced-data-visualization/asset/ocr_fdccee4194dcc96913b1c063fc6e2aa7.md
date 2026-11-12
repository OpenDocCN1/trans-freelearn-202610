# 10. Your worksheet should look as follows:

![](bbox=[0.163, 0.093, 0.894, 0.532])

Figure 9.17: Texas city boundaries

You can see that we have a nice dark map now with polygons on top. The polygons define the area per city name (using the field **City Nm**) as published by the government of Texas. By placing the measure of the population in 2010 (using the field **Pop2010**) on the **Color** shelf, we can also see that the areas with red polygons had the highest number of inhabitants in 2010, and dark green the least.

You might not like the polygons, or maybe you are wondering if a spatial file like a .shp will only allow you to use polygons. Luckily, the answer is that we can change the map, no matter whether it's a spatial file or not. I really like heatmaps to display geographical density, so let's use them in the next section.

# Heatmaps

I want to share a feature with you that was part of an earlier Tableau release and has proven to be very useful when working with geographical data. It is the **Marks** type **Density**, with which you can create heatmaps. This new feature is not limited to maps; you can also use it for any other type of chart. However, it is most efficient for dense data where patterns cannot be spotted easily.