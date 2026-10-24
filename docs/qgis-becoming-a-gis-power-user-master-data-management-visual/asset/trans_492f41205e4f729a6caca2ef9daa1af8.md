
## Leaflet 与外部 API：CartoDB SQL

在第一部分中，我们使用 Leaflet 和包含图层的本地 GeoJSON 文件创建了一个 Web 应用程序。在本部分中，我们将使用 Leaflet 来显示来自外部 API – CartoDB SQL API 的数据。请执行以下步骤：

-   **1.** 打开 **qgis2leaf Export** 对话框（导航至 **Web** | **qgis2leaf** | **Exports**）。
-   **2.** 在 **qgis2leaf** 对话框中，你可以保持输入项为默认值。由于我们将大量修改输出代码，所以这部分并不那么重要。你可以选择添加一个底图；MapQuest Open OSM 对此是一个不错的选择。
-   **3.** 注意输出位置。
-   **4.** 点击 **OK**。
-   **5.** 在输出目录中找到 `index.html`。
-   **6.** 用以下代码替换 `index.html` 的内容（也可在 `c3/data/web/index.html` 中找到）。这段代码与现有的 `index.html` 相同，但有一些修改。第 25 行之后的所有行以及关闭 `script`、`body` 和 `html` 标签下方的行已被删除。添加了 `getToxic` 函数及其调用。查找此函数以用你的 CartoDB 账户名和 API 密钥替换现有的填充文本。此函数执行我们的 CartoDB SQL 查询并显示结果。我们将注释掉第二个函数调用，你可能希望测试它，以查看基于传入的不同坐标对产生的不同结果，如下所示：

```html
<!DOCTYPE html>
<html>
    <head>
        <title>QGIS2leaf webmap</title>

        <meta charset="utf-8" />
        <link rel="stylesheet" href="http://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.3/leaflet.css" /> <!-- we will use this as the styling script for our webmap-->
        <link rel="stylesheet" href="css/MarkerCluster.css" />
        <link rel="stylesheet" href="css/MarkerCluster.Default.css" />
        <link rel="stylesheet" type="text/css" href="css/own_style.css"/>
        <link rel="stylesheet" href="css/label.css" />
        <script src="http://code.jquery.com/jquery-1.11.1.min.js"></script> <!-- this is the javascript file that does the magic-->
        <script src="js/Autolinker.min.js"></script>
    </head>
    <body>
        <div id="map"></div> <!-- this is the initial look of the map. in most cases it is done externally using something like a map.css stylesheet where you can specify the look of map elements, like background color tables and so on.-->
        <script src="http://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.3/leaflet.js"></script> <!-- this is the javascript file that does the magic-->
        <script src="js/leaflet-hash.js"></script>
        <script src="js/label.js"></script>
        <script src="js/leaflet.markercluster.js"></script>

        <script src='data/exp_toxicchannels.js' ></script>

        <script>
            var map = L.map('map', { zoomControl:true }).fitBounds([[39.4194805496,-75.8685268698],[39.9951967581,-75.2662748017]]);
            var hash = new L.Hash(map); //add hashes to html address to easily share locations
            var additional_attrib = 'created with <a href="https://github.com/geolicious/qgis2leaf" target="_blank">qgis2leaf</a> by <a href="http://www.geolicious.de" target="_blank">Geolicious</a> & contributors<br>';
            var feature_group = new L.featureGroup([]);

            var raster_group = new L.LayerGroup([]);

            var basemap_0 = L.tileLayer('http://otile1.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.jpeg', {
                attribution: additional_attrib + 'Tiles Courtesy of <a href="http://www.mapquest.com/">MapQuest</a> &mdash; Map data: &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors,<a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>'
            });
            basemap_0.addTo(map);
            var layerOrder=new Array();
            function pop_toxicchannels(feature, layer) {
            }
            });
            feature_group.addLayer(exp_toxicchannelsJSON);

            layerOrder[layerOrder.length] = exp_toxicchannelsJSON;
            for (index = 0; index < layerOrder.length; index++) {
                feature_group.removeLayer(layerOrder[index]);
                feature_group.addLayer(layerOrder[index]);
            }

            //add comment sign to hide this layer on the map in the initial view.
            exp_toxicchannelsJSON.addTo(map);
            var title = new L.Control();
            title.onAdd = function (map) {
                this._div = L.DomUtil.create('div', 'info'); // create a div with a class "info"
                this.update();
                return this._div;
            };
            title.update = function () {
                this._div.innerHTML = '<h2>This is the title</h2> This is the subtitle'
            };
            title.addTo(map);
            var baseMaps = {
                'MapQuestOpen OSM': basemap_0
            };
            L.control.layers(baseMaps,{"toxicchannels": exp_toxicchannelsJSON},{collapsed:false}).addTo(map);
            L.control.scale({position: 'bottomleft',maxWidth: 100,metric: true,imperial: false,updateWhenIdle: false}).addTo(map);

            /* we've inserted the following after the existing index.html line 83, to handle query to cartodb */

            function getToxic(lon,lat)
            {
                var toxicLayer = new L.GeoJSON();

                $.getJSON(
                "http://YOURCARTODBACCOUNTNAMEHERE.cartodb.com/api/v2/sql?q=%20SELECT%20toxic_channels.*%20FROM%20toxic_channels%20INNER%20JOIN%20channels%20ON%20toxic_channels.join_basin%20=%20channels.basin%20WHERE%20toxic_channels.join_order%20%3C%20(SELECT%20channels._order%20FROM%20channels%20WHERE%20st_distance(the_geom,%20ST_GeomFromText(%27POINT(" + lon + "%20" + lat + ")%27,4326))%20IN%20(SELECT%20MIN(st_distance(the_geom,%20ST_GeomFromText(%27POINT(" + lon + "%20" + lat + ")%27,4326)))%20FROM%20channels%20x))%20AND%20toxic_channels.join_basin%20=%20(SELECT%20channels.basin%20FROM%20channels%20WHERE%20st_distance(the_geom,%20ST_GeomFromText(%27POINT(" + lon + "%20" + lat + ")%27,4326))%20IN%20(SELECT%20MIN(st_distance(the_geom,%20ST_GeomFromText(%27POINT(" + lon + "%20" + lat + ")%27,4326)))%20FROM%20channels%20x))%20GROUP%20BY%20toxic_channels.cartodb_id%20&api_key=YOURCARTODBAPIKEYHERE&format=geojson&callback=?",
                function(geojson) {
                    $.each(geojson.features, function(i, feature)
                    {
                        toxicLayer.addData(feature);
                    })
                });
                map.addLayer(toxicLayer);
            }

            getToxic(-75.56111,39.72583);

            //getToxic(-75.70993,39.69099);
        </script>
    </body>
</html>
```

你的结果应该类似于以下图片：

![](img/2e88bf5938bf48a409c0a534a8775b21_299_0.png)

## 摘要

在本章中，我们通过一个物理栅格分析示例：水文分析，创建了一个动态 Web 应用程序。为此，我们首先准备用于水文分析的栅格高程数据，然后执行了分析。我们探讨了如何使用模型器工作流自动化工具来自动化该工作流。接下来，我们使用 NNJoin 在一些水文输出之间创建空间连接，以生成适合 Web 交互和查询的数据源。最后，我们将这些数据发布到外部云平台 CartoDB，并在 JavaScript 函数中实现其 SQL API，以根据 Leaflet Web 客户端交互，查找位于某地点上游的有毒站点。在下一章中，我们将使用网络分析和众包交互来创建一个 Web 应用程序。

## 4 寻找最佳路径

在本章中，我们将探索正式的类网络地理矢量对象关系。拓扑关系在地理数据管理和分析中有很多用途，但也许最重要的应用是最优路径搜索。具体来说，你将学习如何创建与最优路径相关的几个可视化：等时圈多边形和累积交通线。以这些视觉元素为背景，我们将在我们的 Web 地图应用程序中通过 Twitter 融入社交媒体反馈。最终结果将是一个能与利益相关者就安全上学路线进行双向交流的应用程序。

在本章中，我们将涵盖以下主题：

- 下载 OpenStreetMap 数据
- 空间查询
- 安装 Postgres/PostGIS/pgRouting
- 构建拓扑网络
- DB 管理器
- 使用最短路径插件测试拓扑
- 生成到达每个路段的代价
- 创建等时圈等高线
- 生成所有学生的最短路径
- 通过 Python 添加 Twitter 数据

