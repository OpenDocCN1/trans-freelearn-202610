
![](img/2e88bf5938bf48a409c0a534a8775b21_539_0.png)

## How it works...

Thanks to **Python Console** and the editor, we are not limited to the existing tools and plugins. Instead, we can create our own specialized scripts such as the following one. This script creates a short layer statistics report using HTML and the Google Charts Javascript API (for more information and API docs refer to `https://developers.google.com/chart/`), which it then displays in a `QWebView` window. Of course, you can use any other JavaScript charting API as well. (Note that you need to be connected to the Internet for this script to work because it has to download the Javascript.) We recommend using the editor that was introduced in the previous recipe. Don’t forget to select the layer in the legend:

```
import processing
from PyQt4.QtWebKit import QWebView
layer = iface.activeLayer()
values = []
features = processing.features(layer)
for f in features:
    values.append( f['elev_m'])
myWV = QWebView(None)
html='<html><head><script type="text/javascript"'
html+=' src="https://www.google.com/jsapi"></script>'
html+='<script type="text/javascript">'
html+='google.load("visualization","1",{packages:["corechart"]});'
html+='google.setOnLoadCallback(drawChart);'
```

```
html+='function drawChart() { '
    html+='var data = google.visualization.arrayToDataTable([ '
        html+='["%s"],' % (field_name)
        for value in values:
            html+='[%f],' % (value)
    html+='] );'
    html+='var chart = new google.visualization.Histogram('
        html+='document.getElementById("chart_div"));'
    html+='chart.draw(data, {title: "Histogram"});</script></head>'
    html+=' <body><h1>Layer: %s</h1>' % (layer.name())
    html+=' <p>Values for %s range from: ' % (field_name)
    html+='%d to %d</p> ' % (min(values),max(values))
    html+='<div id="chart_div"style="width:900px; height:500px;">'
    html+='</div></body></html>'
    myWV.setHtml(html)
    myWV.show()
```

Of course, custom reports such as this one lend themselves to adding more details. For example, we can create separate histograms for each POI class or add other types of charts, such as scatter charts:

![](img/2e88bf5938bf48a409c0a534a8775b21_540_0.png)

The first part (lines 1 to 12) is very similar to the script explained in the previous recipe, *Listing unique values in a column:* We get the active layer and collect all elevation values in the values list.

The `QWebView` created on line 14 enables us to display the HTML content, which we then generate in the following section (lines 16 to 34). First, we load the Google Charts Javascript. The actual magic happens in the `drawChart()` function starting on line 21. Lines 22 to 26 create the data object, which is filled with the elevation values from our `values` list. The last three lines of the function (lines 27 to 29) finally create and draw the histogram chart. Finally, lines 30 to 34 contain the HTML body definition with the header stating the layer name and a short introduction text that states the min and max elevation values.

## See also

- For those who want to perform more advanced graphing and numerical analysis, consider using the matplotlib python library or reading your data sources into R. Aggregate functions in SpatialLite or PostGIS can also provide you with min, max, average, sum, and other summarization functions. For PostGIS, refer to `http://www.postgresql.org/docs/9.1/static/functions-aggregate.html`.

## Exploring spatiotemporal vector data using Time Manager

In this recipe, we will look at exploring spatiotemporal vector data using the Time Manager plugin. We'll use event data from the **ACLED (Armed Conflict Location and Event Data Project)** at `http://www.acleddata.com/about-acled/`.

## Getting ready

To follow this recipe, please load `ACLED_africa_fatalities_dec2013.shp`. The layer style that you will see in the following screenshots consists of a simple circle marker at 50% transparency with the data-defined size set to the number of fatalities of the incident. (You can read more about styling in *Chapter 10, Cartography Tips, and Learning QGIS* book by Packt Publishing.) If you want some additional geographic context, you can also load `NE_africa.shp`, which contains the outline of Africa.

## Data Exploration

## How to do it...

Once the data is loaded, all event positions will be displayed. The default way to filter the events, for example, to only see the events from December 1, is to use Layer | Query and enter a filter expression or query, such as the following:

```
"EVENT_DATE" >= '2013-12-01' AND "EVENT_DATE" < '2013-12-02'
```

- 1. It's easy to see that updating this query manually for each day will not be a very convenient way to explore spatiotemporal data. Therefore, we will use the Time Manager plugin (installed using Plugin Manager).
- 2. The Time Manager panel will be added to the bottom of the QGIS window once the plugin is installed. Click on the **Settings** button to open the **Time manager settings** dialog. We can configure Time Manager here.
- 3. Click on **Add Layer** to open the **Select layer and column(s)** dialog.
- 4. Select the **ACLED_africa_fatalities_dec2013** layer and **EVENT_DATE** as starting time and then click on **OK** to add the event point layer to the list of managed layers, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_542_0.png)

> Optionally, you can enable **display frame start time on map** to add a small label with the corresponding timestamp to the rendered map.

5. Click on **OK** when you are done. At this point, Time Manager applies the temporal filter to the dataset, so this can take some time depending on the size of the dataset used.

6. By default, after the first layer has been added, Time Manager will display all the events that occurred during the first day of the dataset. It is easy to adjust the filter by changing the **Time frame size** settings. You can increase the number of days that should be displayed or change to one of the other time units, including seconds, minutes, hours, weeks, and months, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_543_0.png)

7. Once you are happy with the settings, you have multiple options to navigate through time:

- Click on the play button in the bottom-left corner of the Time Manager panel to start an automatic animation
- Move the time slider to the center of the panel like you would do to navigate within a video or music player application
- Click on the forward or backward button on either side of the slider to advance or go back by one time frame
- Of course, you can also edit the **Time frame start** setting directly

## Data Exploration

## How it works...

For performance reasons, Time Manager relies on the layer query/filter expression capability of QGIS. This comes with the following limitations:

- Time Manager can only be used with data sources that support layer queries or filter expressions. Most notably, this means that it cannot be used with delimited text layers.
- As the layer queries or filter expressions have to work with strings, it has to be possible to order the date-time values correctly using text sort. Therefore, the values have to be stored in one of the following formats:

```
%Y-%m-%d %H:%M:%S.%f
%Y-%m-%d %H:%M:%S
%Y-%m-%d %H:%M
%Y-%m-%dT%H:%M:%S
%Y-%m-%dT%H:%M:%SZ
%Y-%m-%dT%H:%MZ
%Y-%m-%d
%Y/%m/%d %H:%M:%S.%f
%Y/%m/%d %H:%M:%S
%Y/%m/%d %H:%M
%Y/%m/%d
%H:%M:%S
%H:%M:%S.%f
%Y.%m.%d %H:%M:%S.%f
%Y.%m.%d %H:%M:%S
%Y.%m.%d %H:%M
%Y.%m.%d
%Y%m%d%H%M%S
```

> If your data uses a different format, which is ordered correctly as well, you can add it to `timevectorlayer.py` or change the format using Field Calculator.

## See also

- The following recipes will show you how to create videos and more sophisticated time-dependent styles using Time Manager.

## Creating animations using Time Manager

In this recipe, we will use the Time Manager plugin to create an image series out of our spatiotemporal QGIS project and turn it into a video, which is ready to be uploaded on Youtube or added into a presentation using easily available and free tools.
