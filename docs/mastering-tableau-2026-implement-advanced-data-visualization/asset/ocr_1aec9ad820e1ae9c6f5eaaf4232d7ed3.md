The second calculated field should be called Destination City and contain the following code:

```
{ FIXED [Trip ID]:MIN(IF [Dest/Orig]='Destination' THEN City END)}
```

+   25. Use the newly created calculated fields to format as desired. In particular, notice that in the following screenshot, **Source City** is on the **Color** shelf and **Destination City** is used on the **Tooltip** shelf:

![](bbox=[0.17, 0.19, 0.83, 0.51])

# Figure 9.7: Flights

To sum up what we have done here, initially, we added a dataset to Tableau. We then needed to copy the latitudes and longitudes per city out of the workbook in order to create a second data source, which we then used as a blend to look up latitude and longitude values. With the help of LOD calculations, we could identify the source and destination cities, and lastly, we were able to measure the distance between cities by using the so-called great circle formula. Each color in the visualization represents a starting city; for example, all green lines measure distances from Perth to other cities. By hovering over a line, the source and destination city, as well as the distance measured in miles (or kilometers), are shown.

If you want to analyze shipment routes or any other distances between two points, you now know exactly how to visualize this kind of data in Tableau.