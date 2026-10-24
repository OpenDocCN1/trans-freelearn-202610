
- 处理地理空间数据：
  - 地理空间分析软件：
    - ArcGIS
    - QGIS
    - ogr2ogr
  - PostGIS 空间函数
  - 地理空间与大数据
- 解决污染报告问题

## 为什么物联网需要地理空间分析？

设想你的公司销售一种测量空气污染物的设备。它支持联网，并使用 MQTT 协议定期将数据传回公司。该产品的目标市场是具有环保意识的消费者，他们既希望测量自家附近的污染物，也希望为环境的集体监测做出贡献。

其价值主张是：消费者可以免费获得本地空气质量分析，代价是捐出他们的数据，以支持一项他们很可能本来就认同的事业。你的公司计划将高质量空气污染数据的分析结果进行汇总和打包，出售给政府和私人组织。

由于该设备通过多种零售渠道间接销售给消费者，公司最初并不知道设备的位置。消费者购买后会将设备连接到互联网，然后输入自己的地址。此时便可以确定设备位置。

该设备配备多个传感器，用于测量空气中不同污染物的浓度。其中一个传感器测量二氧化氮（NO2）的浓度。NO2 本身不仅是一种有毒气体，还会产生更具破坏性的副作用。它会促进酸雨和光化学烟雾的形成，并且是臭氧等其他有害二次空气污染物的前体。

下图展示了 Aeroqual 公司制造的二氧化氮传感器和二氧化氮分析模块的一个示例：

![](img/9f2a55db1236f995dcd9392acbc82910_249_0.png)

二氧化氮传感器和分析仪。来源：Aeroqual

NO2 是由化石燃料燃烧产生的。在城市地区，主要来源通常是机动车尾气，但该气体也可能来自发电厂、制造设施和焊接作业。

你的公司希望构建并销售一个数据包，根据距州际公路的距离汇总 NO2 水平；同时希望将结果数据按美国 115 个国会选区进行聚合。公司认为，国会游说者会愿意为这些信息支付高价。

这项任务起初可能看似令人生畏，因为你所知道的设备位置仅仅是客户注册的地址。最先想到的可能是人工流程：在地图上逐一查看每个设备的位置，并根据其与最近州际公路的距离进行分类。当你有 50 万台设备时，这将耗费大量人力且成本高昂。

幸运的是，地理空间分析可以高效地完成这类分析。我们将介绍几个概念，然后回到这个示例，展示如何解决它。

所有位置都是相对于某个其他起点来确定的。要精确表示一个位置，你需要知道起点在哪里，以及从该起点确定位置的框架。实现这一点有多种表示方法。这就是 **坐标参考系（CRS）** 发挥作用的地方。

## 坐标参考系

坐标参考系，也称为 **空间参考系（SRS）** 或 **地图投影**，用于标识将类似球体的三维地球转换为平面 X、Y 坐标面时所使用的方法。换句话说，它解释了用何种方法将三维投影到二维表面。

这样的定义方式有成千上万种，每种都有自己的优缺点。每种定义的方法都是一个独立的 CRS，其中最常见的是 **1984 年世界大地测量系统** 或 **WGS 84**，它被 **全球定位系统（GPS）** 所使用。

为了正确确定位置并准确计算诸如两点之间表面距离之类的数据，必须知道一组空间数据的 CRS。它通常会被标识并作为文件或地理空间数据库的一部分进行存储。只要两个 CRS 都已知，就可以相互转换。

## 地球并不是一个球体

世界不是一个完美的球体。它是一个椭球体，而且像中年男人一样，中部隆起。这意味着，对地理空间数据使用简单的球体几何计算不会完全准确。

随着两点间距离的增加，使用计算球面距离的方法（例如用于大圆距离计算的 haversine 公式）会变得越来越不准确。你仍然可以使用 haversine 方法计算短距离，精度损失不大，但应避免用于较长距离。

![](img/9f2a55db1236f995dcd9392acbc82910_251_0.png)

haversine 公式。来源：Wikipedia.org

以下 R 代码可用于计算地球上两点的 haversine 距离。请记住，它在长距离上并不完全准确。相反，应利用了解 CRS 的代码包和数据库系统，它们能够使用 CRS 精确计算空间数据。这段 R 代码使用了相距很远的两个点，让你了解该方法的相对不准确性。两点之间的实际距离为 12,935 千米：

```
#code adapted from RosettaCode.

#Coordinates for the two points
#Chicago, USA O'Hare airport (ORD)
Point1Lat = 41.978194
Point1Long = -87.907739

#Coordinates for Chhatrapati Shivaji International Airport near Mumbai,
India airport (BOM)
Point2Lat = 19.0895595
Point2Long = 72.8656144

#convert decimal degrees to radians
degrees_to_rad <- function(deg) (deg * pi / 180)

# Volumetric mean radius is 6371 km for the Earth, see
http://nssdc.gsfc.nasa.gov/planetary/factsheet/earthfact.html
# The diameter is thus 12742 km

#function to calculate great circle distance using haversine method
great_circle_distance <- function(lat1, long1, lat2, long2) {
a <- sin(0.5 * (lat2 - lat1))
b <- sin(0.5 * (long2 - long1))
12742 * asin(sqrt(a * a + cos(lat1) * cos(lat2) * b * b))
}

#calculate distance for the two points
haversine_distance <- great_circle_distance(
degrees_to_rad(Point1Lat), degrees_to_rad(Point1Long),   # Nashville
International Airport (BNA)
degrees_to_rad(Point2Lat), degrees_to_rad(Point2Long))  # Los Angeles
International Airport (LAX)

#result shown in kilometers
haversine_distance
# 12,942.77km
```

有多种方法可以针对地球的实际形状进行调整。WGS 84 CRS 使用了一些参数来提高投影的精度。下表总结了这些参数：

| 参数 | 符号 | 值 |
|---|---|---|
| 地球扁率因子 | 1/f | 298.257223563 |
| 地心引力常数 | GM | 3986004.418 10^8 m³/s² |
| 标称平均角速度 | ω | 7292115 10^-11 rad/s |
| 半长轴 | a | 6378137.0 m |

WGS 84 定义参数。来源：联合国外层空间事务厅

尽管你接触到的 WGS 84 CRS 坐标大多是二维的，但它实际上是地球的三维表示。起点基于地球质心。下图来自国防测绘局（Defense Mapping Agency），展示了其表示方式：

![](img/9f2a55db1236f995dcd9392acbc82910_253_0.png)

WGS 参考框架，来自国防测绘局的 DMA 技术报告 TR8350.2-b（1987 年 12 月 1 日第二次印刷）第 1-5 节 PDF，作为 DoD WGS 84 技术报告第 2 部分（参数、公式和图形）的补充。http://earth-info.nga.mil/GandG/publications/tr8350.2/TR8350.2-b/Sections%201-201-5.pdf. 公共领域，https://commons.wikimedia.org/w/index.php?curid=41796676

强调这一细节不仅是为了让你了解地图投影背后的复杂性，更是为了说服你利用该领域数百年来已经完成的工作。不要试图重复这一工作；要利用已有的成果。

利用地理空间分析领域已有成果的一个好方法是使用 Python 地理空间库。R 语言也有很多优秀的包可以使用，但 Python 的选择更多。由于 Python 在地理空间界长期使用，这些库往往更加成熟。

Python 通常也比 R 具有更好的扩展性，因此更适合大规模、计算密集型的处理。地理空间计算可能变得相当密集，而物联网数据——正如我们在本书中已经熟知的那样——会在短时间内变成大规模任务。

基于这些考虑，Python 非常适合地理空间分析。在本章中，我们将把重点从 R 代码转向 Python 代码。对于大数据分析，你应该对两者都熟练掌握。开始使用 Python 的一个简单方法是从 https://www.continuum.io/downloads 下载 Anaconda 包。Anaconda 包含 Python、R 以及超过 720 个包，其中包括 100 多个最流行的数据科学包。

它还包含一个名为 Spyder 的 Python IDE 和基于浏览器的笔记本 Jupyter。Jupyter 对物联网分析很重要，因为它可以在 Hadoop 集群上运行，让你能够在分布式环境中开发 Python 代码，而无需通过控制台运行代码。它还可以用于以 Python、Scala 或 R 开发 Spark 应用程序。Spark 与跨集群安装的 Python 地理空间库相结合，提供了扩展地理空间分析的能力。

## 基于矢量的方法

地理空间分析和文件类型主要有两大类：矢量和栅格。矢量主要处理形状，而栅格则更侧重于网格。由于灵活性和高效存储，矢量更为常见。矢量可以简单地用一组点来定义。矢量几何有三种主要类型：

- **点：** 可以用二维或三维来定义。这就是你可能非常熟悉的常用经纬度坐标对。前面 R 代码中使用的机场位置就是点的示例。
