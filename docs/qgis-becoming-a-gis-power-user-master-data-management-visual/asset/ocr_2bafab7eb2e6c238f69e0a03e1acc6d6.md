# Data Exploration

# How to do it...

Once the data is loaded, all event positions will be displayed. The default way to filter the events, for example, to only see the events from December 1, is to use Layer | Query and enter a filter expression or query, such as the following:

```
"EVENT_DATE" >= '2013-12-01' AND "EVENT_DATE" < '2013-12-02'
```

+   1. It's easy to see that updating this query manually for each day will not be a very convenient way to explore spatiotemporal data. Therefore, we will use the Time Manager plugin (installed using Plugin Manager).
+   
+   2. The Time Manager panel will be added to the bottom of the QGIS window once the plugin is installed. Click on the **Settings** button to open the **Time manager settings** dialog. We can configure Time Manager here.
+   
+   3. Click on **Add Layer** to open the **Select layer and column(s)** dialog.
+   
+   4. Select the **ACLED_africa_fatalities_dec2013** layer and **EVENT_DATE** as starting time and then click on **OK** to add the event point layer to the list of managed layers, as shown in the following screenshot:

![](bbox=[0.216, 0.446, 0.785, 0.718])

> Optionally, you can enable **display frame start time on map** to add a small label with the corresponding timestamp to the rendered map.