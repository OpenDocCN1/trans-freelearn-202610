

## 处理点、线和多边形

显然，构成 Shapefile 的所有 airports.* 文件都需要位于同一目录（本例中为 C:\Data 目录）中，图层才能被正确读取。

### 线

线和多边形图层在两个方面比点图层更复杂。首先，单独的线或多边形由一组点定义，而不是单个点。具体来说，线是一组从第一个点连接到最后一个点的点。多边形的定义类似，只是最后一个点与第一个点相同。每条线或多边形中的点数不是固定的，而是取决于形状的复杂度（更复杂的形状将由更多点来定义）。其次，单个线或多边形要素（对应属性表中的单个条目）可以由多个单独的线或多个单独的多边形组成。例如，在某个国家边界图层中，美国要素将由多个多边形组成，因为美国本土、阿拉斯加以及夏威夷各岛彼此分离。

![](img/ebfd6294770b225fdeceda9a7b8b560e_153_0.png)

> 另一个特定于多边形图层的复杂情况是空洞的存在。单个要素可以由外部边界多边形和空洞多边形组成。例如，在北美陆地面积图层中，空洞多边形可能代表五大湖。空洞多边形内部又可以有内部多边形（例如五大湖中的岛屿），依此类推。

由于其复杂性，手动从原始坐标定义线和多边形图层的情况要少得多。相反，它们通常要么从外部来源导入（例如读取 Shapefile），要么由其他对象创建（例如基于 DEM 栅格创建的等高线）。

作为 `SpatialLinesDataFrame` 的示例，我们将读取一个包含来自 GPS 设备的轨迹记录的 GPS 交换格式（GPX）文件。与 Shapefile 不同，对于 GPX 文件，`dsn` 参数是文件本身，而 `layer` 参数指向我们想要读取的数据组件（例如 `"tracks"` 或 `"track_points"`；参见 `?readOGR`）。特定的文件 `GPS_log.gpx` 包含 GPS 日志，记录的是在记录模式下驾驶 GPS 设备绕行 Lahav 和 Dvira 森林的经过。我们如下读取该文件：

```r
> track = readOGR("C:\\Data\\GPS_log.gpx","tracks")
```

使用 `class` 函数，我们可以证明 `track` 是一个 `SpatialLinesDataFrame` 对象：

```text
> class(track)
[1] "SpatialLinesDataFrame"
attr(,"package")
[1] "sp"
```

### 多边形

作为多边形数据的示例，我们将读取另一个外部文件；这次是一个名为 `USA_2_GADM_fips` 的 Shapefile。该文件由对应于美国第二级行政区划（县）的多边形组成，并带有多个属性，例如县名和**联邦信息处理标准（FIPS）**代码。我们将生成的 `SpatialPolygonsDataFrame` 赋给名为 `county` 的对象，以便在本章后续示例中使用：

```r
> county = readOGR("C:\\Data", "USA_2_GADM_fips",
+ stringsAsFactors = FALSE)
```

总而言之，到目前为止，我们见证了在 R 中创建空间矢量图层的两种方法：从一组 (x,y) 坐标（适用于点）和从文件（适用于点、线和多边形）。第三种方法——从栅格数据派生矢量图层——将在*第 7 章“组合矢量与栅格数据集”*中介绍。

### 探索矢量图层属性与子集化

本节将致力于检查空间矢量图层的属性，并根据其属性表进行子集化。所介绍的一些操作将与上一章中针对栅格介绍的操作类似（例如绘图和查询 CRS 信息），而另一些操作通常仅与矢量图层相关（例如计算面积和根据属性表创建子集）。很快就会发现，许多涉及矢量图层属性表的操作与对 `data.frame` 对象的操作方便地类似。

#### 检查矢量图层属性

summary 函数会生成关于矢量图层属性的有用文本摘要，包括其类、边界框坐标、CRS 和属性表列类型。例如，对 airports 使用 summary 会产生以下文本输出：

```text
> summary(airports)
Object of class SpatialPointsDataFrame
Coordinates:
                min         max
lon -106.79467 -106.07308
lat    35.04918    35.62866
Is projected: FALSE
proj4string : [+proj=longlat +datum=WGS84]
Number of points: 3
Data attributes:
      Length      Class      Mode
          3 character character
```

此输出中列出的所有属性也可以使用函数访问，在某些情况下还可以修改。例如，与我们在上一章中已经看到的栅格数据类似，`proj4string` 函数返回矢量图层的 PROJ.4 格式 CRS 定义。对 airports 使用 `proj4string` 会返回 WGS84 CRS 的定义：

```text
> proj4string(airports)
[1] "+proj=longlat +datum=WGS84"
```

关于几何部分，`length` 函数返回图层包含的要素数量。例如，airports 包含三个点（三个机场），如下输出所示：

```text
> length(airports)
[1] 3
```

空间图层也总是有行名，这些行名在内部用作 ID 变量，用于将几何与属性表条目匹配。因此，行名的数量等于要素的数量：

```text
> row.names(airports)
[1] "1" "2" "3"
```

`dimensions` 函数返回空间维度的数量：

```text
> dimensions(airports)
[1] 2
```

> 在本书中，我们只处理二维矢量图层（平面上的几何）。三维图层也可以用于表示某些类型的数据，例如具有 (x,y) 坐标和 (z) 高程的点。

#### 访问矢量图层的属性表

矢量图层的属性表实际上是一个 `data.frame` 对象，一些适用于 `data.frame` 对象的函数已经被定义，以便也能直接一致地应用于矢量图层。例如，应用于矢量图层的 `nrow`、`ncol` 和 `dim` 函数将参照其属性表返回维度：

```text
> nrow(county)
[1] 3145
> ncol(county)
[1] 4
> dim(county)
[1] 3145    4
```

我们看到 `county` 的属性表有 3,145 行（因此该图层有 3,145 个要素）和四列。这些列包含以下信息：

- `NAME_1`：第一级名称（例如州名）
- `NAME_2`：第二级名称（例如县名）
- `TYPE_2`：要素类型（例如 “County” 或 “Water body”）
- `FIPS`：FIPS 代码

属性表的单个列或其子集可以使用 `$` 和 `[` 运算符访问。例如，`county` 中前 10 个要素的第二级名称（位于 `NAME_2` 列）可以按如下方式获得：

```text
> county$NAME_2[1:10]
[1] "Litchfield" "Hartford"    "Tolland"    "Windham"
[5] "Siskiyou"  "Del Norte"   "Modoc"       "New London"
[9] "Fairfield" "Middlesex"
```

再举一个例子，我们可以通过列出 `TYPE_2` 列中的唯一值来检查 `county` 图层包含的要素类型：

```text
> unique(county$TYPE_2)
  [1] "County"          "District"          "Borough"
  [4] "Census Area"     "Municipality"      "City And Borough"
  [7] "City And County" "Water body"        "Parish"
 [10] "Independent City"
```

空间矢量图层的整个属性表可以直接使用 `@` 运算符访问。`@` 运算符用于按名称从对象中提取槽，使用 `对象名@槽名` 的表示形式。

![](img/ebfd6294770b225fdeceda9a7b8b560e_157_0.png)

> 更具体地说，`@` 运算符适用于所谓的 S4 类对象，我们处理的所有栅格和矢量图层都是 S4 类；而 S3 类对象的组件则使用不同的方法（`$` 运算符）访问。S3 与 S4 的区别涉及内部类结构，超出了本书的讨论范围。更多信息请参阅《Advanced R》，Wickham, H., CRC Press, 2014（http://adv-r.had.co.nz/OO-essentials.html）。

sp 包中定义的空间矢量类的属性表槽被称为 `data`。因此，在矢量图层名称后添加 `@data` 将返回其属性表（如果有的话）。

例如，以下表达式返回 `airports` 的属性表：

```text
> airports@data
                name
1 Albuquerque International
2          Double Eagle II
3       Santa Fe Municipal
```

另一个例子，我们可以使用 `head` 函数应用于 `county@data` 来打印 `county` 属性表中的前几行：

```text
> head(county@data)
        NAME_1      NAME_2 TYPE_2  FIPS
0 Connecticut Litchfield County 09005
1 Connecticut   Hartford County 09003
2 Connecticut    Tolland County 09013
3 Connecticut   Windham County 09015
4 California   Siskiyou County 06093
5 California  Del Norte County 06015
```

