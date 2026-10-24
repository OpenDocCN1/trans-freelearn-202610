

# 将地理空间分析应用于物联网数据

栅格和矢量数据层可以组合使用。以下图片由 NOAA 提供，展示了它们如何共同用于现实世界的表示：

![](img/9f2a55db1236f995dcd9392acbc82910_262_0.png)

## 存储地理空间数据

存储地理空间数据的方式有很多种。根据你的预期用途，文件系统格式或关系数据库可能是最合适的。我们将对两者进行介绍。

## 文件格式

存储地理空间数据的格式有数百种。矢量数据最常见的是 ESRI shapefiles。shapefile 实际上由多个不同文件组成，主文件扩展名为 .shp。当提供 .shp 文件的位置时，大多数支持地理空间的软件和 Python 包都知道去查找其他所需文件。

**GeoJSON** 是另一种可读的人类存储格式。它使用定义的 JSON 格式将矢量数据定义存储为文本。它易于阅读，但大小可能会变得很大。

另一种表示矢量数据的方式，无论是在文件中还是在代码中，都是使用 **Well-known text (WKT)** 和 **Well-known binary (WKB)** 格式。WKT 可读，而 WKB 不可读。WKB 在大小上提供了显著的压缩，因此通常是数据库存储的良好选择。它可以在读取时转换为 WKT。下面的表格总结了每种几何类型的 WKT 格式：

| Geometry primitives (2D) |
|---|---| 
| Type | Examples |
| Point | POINT (30 10) |
| LineString | LINESTRING (30 10, 10 30, 40 40) |
| Polygon | POLYGON ((30 10, 40 40, 20 40, 10 20, 30 10)) |
| Polygon | POLYGON ((35 10, 45 45, 15 40, 10 20, 35 10), (20 30, 35 35, 30 20, 20 30)) |

| Multipart geometries (2D) |
|---|---| 
| Type | Examples |
| MultiPoint | MULTIPOINT ((10 40), (40 30), (20 20), (30 10)) |
| MultiPoint | MULTIPOINT (10 40, 40 30, 20 20, 30 10) |
| MultiLineString | MULTILINESTRING ((10 10, 20 20, 10 40), (40 40, 30 30, 40 20, 30 10)) |
| MultiPolygon | MULTIPOLYGON (((30 20, 45 40, 10 40, 30 20)), ((15 5, 40 10, 10 20, 5 10, 15 5))) |
| MultiPolygon | MULTIPOLYGON (((40 40, 20 45, 45 30, 40 40)), ((20 35, 10 30, 10 10, 30 5, 45 20, 20 35), (30 20, 20 15, 20 25, 30 20))) |

Well-known Text format examples. Source: Wikipedia

栅格数据最常见存储在 **Tagged Image File Format (TIFF)** (.tiff) 文件中。它也可以存储为 ASCII 网格文件，但文件大小是个问题。还有一些压缩格式，例如扩展名为 .sid 的 **Multi-resolution Seamless Image Database (MrSID)** 和扩展名为 .ecw 的 **Enhanced Compression Wavelet (ECW)**。

## 关系数据库的空间扩展

借助空间扩展，关系数据库可以支持在数据库表中存储几何数据并执行一些地理空间函数。这些通常不是标准安装的一部分，但可以通过管理设置启用或通过安装软件扩展来启用。

对于开源关系数据库管理系统 RDBMS，PostgreSQL 和 MySQL 都支持空间功能。PostgreSQL 是迄今为止最受欢迎的，也是功能最全面的。当为 PostgreSQL 启用空间组件时，通常被称为 PostGIS。你会看到这些术语可以互换使用。PostgreSQL 是 AWS 上受支持的 RDS 选项。空间扩展可以启用，将其转变为 PostGIS。

对于闭源 RDBMS，Oracle (Spatial and Graph) 和 SQL Server 很受欢迎。Oracle 通常被认为是最有能力的。这些并不是唯一的选择，因为越来越多的数据库支持空间数据。Amazon Aurora，一种在 AWS 上的 MySQL 兼容托管 RDS 数据库，最近已添加了空间支持。

## 在 HDFS 中存储地理空间数据

HDFS 和 Hive 本身并不原生支持空间数据类型。不过一切都还没完，因为 HDFS 可以存储任何类型的文件，包括地理空间文件。几何图形可以以字符串 WKT 和二进制 WKB 形式存储。它们可以在检索时使用代码转换。Hive 表是模式即读，并支持用户定义函数 UDF。可以创建 UDF 来解释地理空间数据。

事实上，有一些开源项目正是这样做的。其中一个名为 SpatialHadoop，可以在 http://spatialhadoop.cs.umn.edu/index.html 找到。另一个名为 spatial-framework-for-hadoop，可以在 GitHub 上找到 (https://github.com/Esri/spatial-framework-for-hadoop)。缺点是这些项目没有完全支持，并且不是 Cloudera 和 Hortonworks Hadoop 发行版的一部分。

更稳健的方法是以 WKT 或 WKB 存储空间数据，并使用地理空间 Python 包来操作它。

## 空间索引

世界很大。想象一下，如果你不知道某人的家庭住址、邮政编码，甚至他们居住的国家，要找到他们住在哪里。你必须拜访每一户人家，直到找到你要找的人，这将花费你剩余的时间，而且也不会很有趣。

幸运的是，地址允许快速识别某人居住地，通过识别国家、该国内的州或省、邮政编码以及可以开车前往的街道名称，直到找到他们的门牌号，门牌号通常沿街道按既定顺序排列。

空间数据库可能非常庞大，因此需要一种高效的几何搜索方法来提高响应时间。这就是空间索引发挥作用的地方。有多种方法可用于此。我们将介绍其中一种更流行的方法。

## R-tree

R-tree 是一种空间索引方法，用于 PostGIS 和 Oracle 数据库。它利用边界框概念创建分层索引树。树是平衡的，因为所有分支都有相同级别的节点。为了理解基本 R-tree 索引是如何构建的，我们将通过一个简单示例进行说明。

对于给定的几何集合，每个几何的 MBR 都会被定义。索引中保留的是每个的 MBR。以下图片显示了这些第一步的示例：

![](img/9f2a55db1236f995dcd9392acbc82910_265_0.png)

MBR for a set of shapes. Source: OSGeo project

R-tree 索引由 MBR 构建，使得较大的边界框包含一组较小的边界框，目标是优化最低层 MBR 的搜索时间。由此产生的边界框集合形成树层次结构，作为索引存储。然后使用索引树通过沿层次结构向下工作来快速找到匹配的几何。以下图片有助于可视化示例 R-tree 索引：

![](img/9f2a55db1236f995dcd9392acbc82910_266_0.png)

R-tree index, Source: Wikipedia

像 PostGIS 这样的空间数据库可以轻松地对几何字段创建索引。以下 PostGIS 的简单 SQL 语句将构建 R-tree 索引。PostGIS 在 **Generalized Search Tree (GiST)** 层之上构建它以实现稳健性。GiST 是一种通用算法，可与多种索引方法一起使用。GiST 的示例显示在以下代码中：

```
```sql
CREATE INDEX [indexname] ON [tablename] USING GIST ( [geometryfield] );
```
```

还有一个名为 *rtree* 的 Python 包，可用于在代码模块的一部分中构建索引。这对于一些需要反复扫描一组几何图形的重型地理空间处理可能很有用。

## 处理地理空间数据

专用软件可以帮助处理和可视化地理空间数据。这对于小数据和一次性分析很有用。即使你有大数据解决方案，使用这些工具也可以更有效地向他人传达你的发现。

## 地理空间分析软件

我们将回顾最受欢迎的 **Geographic Information System (GIS)** 工具，以便你熟悉它们。它们是地理空间分析的有用支持工具。

## ArcGIS

ArcGIS 是付费 GIS 软件的事实标准。它由 ESRI 公司开发和维护。它具有令人惊叹的功能量，被大多数专业地理空间分析师使用。它由 ESRI 提供世界级支持，并且有许多培训选择。它链接到有用的数据集和地理空间分析能力，这些也由 ESRI 维护。

