

## 比例尺

比例尺允许你将真实数据的定义域映射到值域，例如，假设你有一组 1 到 100 的值，以及一个可能取值的范围，并希望将所有值映射到 1 到 5 的数字。比例尺在映射中最有用的用途是将一组数值与一组颜色关联起来。以下代码将一组值映射到一组颜色，并将中间值映射为过渡颜色：

```
var color = d3.scale.linear()
    .domain([- .5, 0, 2.66])
    .range(["#FFEBEB", "#FFFFFF", "#E6FFFF"]);
```

## 绑定

数据对象被解析到 DOM 后，可以通过其 `data` 或 `datum` 属性绑定到 D3 对象。

## Select、Select All、Enter、Return、Exit、Insert 和 Append

为了选择可能已存在的元素，你将使用 Select 和 Select All 关键字。然后，根据你预期元素是否已经存在，你将使用 Enter（如果尚不存在）、Return（如果已存在）以及 Exit（如果你希望移除它）关键字来改变与元素的交互。

以下是一个使用 Enter 关键字的 Select All 示例。来自此前解析过的 house_district JSON 的数据，通过 path 元素的 `d` 属性加载，并赋给路径形状生成器。此外，在 `fill` 属性上设置了一个函数，该函数从线性颜色比例尺返回一种颜色：

```
map.selectAll("path")
    .data(topojson.feature(phila, phila.objects.house_district)
        .features)
    .enter()
    .append("path")
    .attr("vector-effect","non-scaling-stroke")
    .style("fill", function(d) { return color
        (d.properties.d_avg_change); })
    .attr("d", path);
```

## main.js

以下所有 JavaScript 代码都位于 `./js/main.js` 中。我们对此代码的所有自定义都将在该文件中完成：

```
var width = 960,
    height = 600;

//sets up the transformation from map coordinates to DOM
  coordinates
var projection = d3.geo.mercator()
    .center([-75.166667,40.03])
    .scale(60000);

//the shape generator
var path = d3.geo.path()
    .projection(projection);

var svg = d3.select("#map-container").append("svg")
    .attr("width", width)
    .attr("height", height);

var g = svg.append("g");

g.append( "rect" )
    .attr("width",width)
    .attr("height",height)
    .attr("fill","white")
    .attr("opacity",0)
    .on("mouseover",function(){
      hoverData = null;
      if ( probe ) probe.style("display","none");
    })

var map = g.append("g")
    .attr("id","map");

var probe,
    hoverData;

var dateScale, sliderScale, slider;

var format = d3.format(",");

  var months = ["Jan"],
```

```
```javascript
.append("div")
.attr("id","loader")
.style("top",d3.select("#play").node().offsetTop + "px")
.style("height",d3.select("#date").node().offsetHeight +
    d3.select("#map-container").node().offsetHeight + "px");

//load and parse whites.csv
d3.csv("csv/whites.csv",function(data){
  var first = data[0];
  // get columns
  for ( var mug in first ){
    if ( mug != "name" && mug != "lat" && mug != "lon" ){
      orderedColumns.push(mug);
    }
  }

  orderedColumns.sort( sortColumns );

  // draw city points
  for ( var i in data ){
    var projected = projection([ parseFloat(data[i].lon),
      parseFloat(data[i].lat) ])
    map.append("circle")
      .datum( data[i] )
      .attr("cx",projected[0])
      .attr("cy",projected[1])
      .attr("r",1)
      .attr("vector-effect","non-scaling-stroke")
      .on("mouseover",function(d){
        hoverData = d;
        setProbeContent(d);
        probe
          .style({
            "display" : "block",
            "top" : (d3.event.pageY - 80) + "px",
            "left" : (d3.event.pageX + 10) + "px"
          })
      })
      .on("mouseout",function(){
        hoverData = null;
        probe.style("display","none");
      })
  }
})
```
```

```
createLegend();

dateScale = createDateScale(orderedColumns).range([0,3]);

createSlider();

d3.select("#play")
    .attr("title","Play animation")
    .on("click",function(){
        if ( !isPlaying ){
            isPlaying = true;
            d3.select(this).classed("pause",true).attr
                ("title","Pause animation");
            animate();
        } else {
            isPlaying = false;
            d3.select(this).classed("pause",false).attr
                ("title","Play animation");
            clearInterval( interval );
        }
    });

drawMonth( orderedColumns[currentFrame] ); // initial map

window.onresize = resize;
    resize();

d3.select("#loader").remove();
    })
});
```

## 输出

最终产品——你可以在网络浏览器中打开 `index.html` 来查看——是一个由时间线控制的动态点集，展示了按人口普查区划分的白人人口变化。该数据叠加显示在众议院选区之上，其颜色根据每年白人人口的变化从冷色到暖色渐变，并在三个变化时期（2010-11、2011-12 和 2012-13）上取平均值。我们的地图应用输出带有时间线动画，效果类似于下图：

![](img/2e88bf5938bf48a409c0a534a8775b21_363_0.png)

## 6

## 估计未知值

在本章中，我们将使用插值方法，根据其他位置的已知值来估计某一位置的未知值。

插值是一种完全基于未知位置与已知位置之间的地理关系来估计未知值的技术。由于空间可以被无限精度地测量，数据测量总是受到数据收集者有限资源的制约。插值和其他更复杂的空间估计技术，可用于估计那些尚未被测量位置的值。在本章中，你将学习如何对气象站数据中的值进行插值，这些值将被评分并用于一个针对特定农业条件——霉病——的脆弱性模型中。我们将气象数据做了一个子集，以提供一年中历史上脆弱性通常较高的月份。最终用户可以使用此应用对模型进行地面真值验证，即将预测的高或低脆弱性与霉病存在与否进行匹配。如果模型扩展到历史数据或接近实时，该应用可用于观察脆弱性随时间的趋势，或提示种植者需要采取行动预防霉病。包括降水量、相对湿度和温度在内的参数，已被选用于预测田间和作物霉病脆弱性的实际模型中。

在本章中，我们将介绍以下主题：

- 从 MySQL 添加数据
- 使用 NetCDF 多维数据格式
- 插值未知值以用于可视化和报告
- 应用简单的代数风险模型
- 使用 Python GDAL 包装器通过 SQLite 查询进行过滤和更新
- 插值
- 地图代数建模
- 用网格点图层对栅格网格进行采样
- Python CGI 托管
- CGI 开发过程中的测试与调试
- Python SpatiaLite/SQLite3 包装器
- 使用 QGIS 插件生成 **OpenLayers3 (OL3)** 地图
- 为 OL3 地图添加 AJAX 交互
- OL3 像素弹出窗口中的动态响应

## 导入数据

通常，用于高度交互式动态 Web 应用的数据存储在企业现有数据库中。尽管这些并非通常意义上的空间数据库，但它们包含坐标位置，可以很容易地在空间应用中被利用。

## 在 QGIS 中连接并从 MySQL 导入

以下部分仅作为演示提供——数据库安装和设置对于简短的使用演示来说过于耗时。

> 如果你确实希望安装并设置 MySQL，可以从 http://dev.mysql.com/downloads/ 下载。MySQL Community Server 在开源 GPL 许可下免费提供。你可能需要安装 MySQL Workbench 和 MySQL Utilities，它们也可在此位置获得，以便与新的 MySQL Community Server 实例进行交互。然后，你可以使用 MySQL Workbench 中的数据导入/恢复命令，并配合提供的备份文件（c6/original/packt.sql）来恢复本演示所用的数据库。

