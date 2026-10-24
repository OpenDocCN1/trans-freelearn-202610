

## 使用 pgRouting Layer 插件进行测试

让我们使用 pgRouting Layer 插件来测试到目前为止我们执行的操作是否生成了一个可工作的拓扑网络，以便查找最短路径。我们将查找网络上两个任意点 1 和 1000 之间的最短路径。请执行以下步骤：

- 1. 安装 pgRoutingLayer 插件。
- 2. 如果未显示最短路径面板，请在 视图 | 面板 | pgRouting Layer 下将其打开。
- 3. 输入以下参数：
  - 数据库：packt_c4。
  - 确保您已连接到数据库，如上一节所示。您可能需要重启 QGIS 才能在此处显示新的数据库连接。
  - edge_table：newark_osm
  - geometry：geom
  - id：id
  - source：source
  - target：target
  - cost：length_m
  - source_id：1
  - target_id：1000

您的输出将类似于以下图像：

![](img/2e88bf5938bf48a409c0a534a8775b21_314_0.png)

## 寻找到达目的地的最佳方式

8. 点击 **Load now!** 按钮，如下图所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_316_0.png)

现在，您可以按照从该位置到学校所需的时间对线段进行符号化。为此，请使用 `traveltime_min` 字段配合 **Graduated**（分级）样式类型。您将看到，数值较低的线段（表示通行速度较快）更接近顶点 1，而数值较高的线段则相反。该方法受网络对真实条件建模程度的限制；例如，铁路会与其他的路段一起显示通行时间。然而，铁路可能会在我们的网络中造成不连续性——因为学生上学时无法“通行”铁路。

![](img/2e88bf5938bf48a409c0a534a8775b21_317_0.png)

## 创建等时线多边形

接下来，我们将创建多边形以可视化学生可以在特定时间范围内步行到达学校的区域。我们可以使用此技术来表征一般的通行时间，同时隐藏学生的具体位置。

### 将通行时间线转换为点

我们需要首先利用多边形作为中间步骤，将当前的基于线的通行时间图层转换为点（质心）。请执行以下步骤：

- 1. 将查询图层保存为 shapefile：`c4/data/output/newark_isochrone.shp`。
- 2. 导航至 **Vector | Geometry Tools | Line to polygons**（矢量 | 几何工具 | 线转面）。输入以下参数：
  - **输入线矢量图层：** isochron lines
  - **输出多边形 shapefile：** `c4/data/output/isochron_polygon.shp`
  - 点击 **OK**
- 3. 导航至 **Vector | Geometry Tools | Polygons to centroid**（矢量 | 几何工具 | 面转质心）。输入以下参数：
  - **输入多边形矢量图层：** `c4/data/output/isochron_polygon.shp`
  - **输出点 shapefile：** `c4/data/output/isochrons_centroids.shp`
  - 点击 **OK**

### 选择点中的通行时间范围并创建凸包

接下来，我们将为每个时间区间创建实际的等时线多边形。我们必须使用过滤表达式为三个时间段选择每组通行时间点：15 分钟或更短、30 分钟或更短，以及 45 分钟或更短。然后，我们将对每个选集运行 **Concave hull**（凹包）工具。这将在每组点周围创建一个多边形特征。

对于每个三个断点值（15、30 和 45），您都需要执行以下三个步骤：

- 1. 从 **Layers**（图层）面板中选择 **isochron_centroids**。
- 2. 导航至 **Layer | Query**（图层 | 查询）。
- 3. 如果查询对话框的过滤表达式字段中已显示过滤表达式，请点击 **Clear**（清除）。
- 4. 提供特定的字段表达式：`cost < [break value]`（例如，`cost < 15`）。
- 5. 点击 OK 以选择图层中匹配该表达式的对象。
- 6. 导航至 **Processing Toolbox | Concave hull**（处理工具箱 | 凹包）。
- 7. 输入 **Concave hull** 的以下参数。其他所有参数均可保持默认值：
  - **输入点图层：isochron_centroids**
  - 选择 **Split multipart geometry into singleparts geometries**（将多重几何拆分为单一部分几何）
  - **Concave hull**（输出文件）可以是 `c4/data/output/isochrone45.shp`
  - 点击 Run（运行），如下图所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_319_0.png)

显示所有凹包时，其外观将类似于以下图像。凹包的“尖刺程度”反映了用于计算这些通行时间多边形的道路线段（点）相对较少：

![](img/2e88bf5938bf48a409c0a534a8775b21_320_0.png)

## 为所有学生生成最短路径

到目前为止，我们只查看了城市中所有给定道路线段之间的最短路径。现在，给定学生位置，让我们看看学生流量会累积在哪里。

### 查找与学生位置相关的线段

通过以下步骤，我们将把最近道路线段的属性——包括相关的拓扑和通行属性——合并到每个学生位置中。请执行以下步骤：

- 1. 安装 NNJoin 插件。
- 2. 导航至 **Plugins | NNJoin | NNJoin**（插件 | NNJoin | NNJoin）。
- 3. 输入以下参数：
  - **输入矢量图层：** **students**
  - **连接矢量图层：** **newark_osm**
  - **输出图层：** `students_topology`
- 4. 点击 **OK**。
- 5. 使用 Database Manager（数据库管理器）将 `students_topology` 导入 `packt_c4` 数据库。

以下图像显示了输入到 NNJoin 插件中的参数：

![](img/2e88bf5938bf48a409c0a534a8775b21_322_0.png)

### 按线段计算累积最短路径

我们要找出给定学生位置、网络特征和学校位置的情况下，哪些路线最热门。以下步骤将生成沿每个网络线段的学生流量累积计数：

- 1. 转到 **Database | DB Manager | DB Manager**（数据库 | DB Manager | DB Manager）。
- 2. 选择要更新的数据库。
- 3. 导航至 **Database | SQL window**（数据库 | SQL 窗口）。
- 4. 我们要运行一个 SQL 命令，为每个学生执行最短路径计算，并找到在每条道路线段上通行的学生总数。此查询可能非常慢。输入以下代码：
```
SELECT id, geom, count(id1)
FROM
(SELECT *
    FROM pgr_kdijkstraPath(
'SELECT id, source, target, traveltime_min as cost FROM newark_osm',
    1, (SELECT array_agg(join_target) FROM students_topology),
    false, false
) a,
    newark_osm b
WHERE a.id3=b.id) x
GROUP BY id, geom
```
- 5. 选择 Load as new layer（作为新图层加载）选项。
- 6. 选择 id 作为具有唯一整数值列，geom 作为几何列，如下图所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_323_0.png)

## 流量符号化

我们希望以一种真正突出显示学生数量较多的线段的方式来可视化每条线段上的学生人数。使用符号化表达式来实现这一点效果很好。这会产生与其他 GIS 软件包中常见的分级符号。请执行以下步骤：

- 1. 导航至 **Layer | Properties | Style**（图层 | 属性 | 样式）。
- 2. 点击 **Simple line**（简单线）以访问符号化表达式，如下图所示：
- 3. 在 **Pen width**（笔宽）部分，点击高级菜单以编辑符号化表达式。

![](img/2e88bf5938bf48a409c0a534a8775b21_324_0.png)

4. 对数函数（Natural log）是一个很好的函数，当数值呈指数增长时，它有助于获得更线性的增长率。这有助于我们产生一种视觉上更美观的符号化变化方式。在 **Expression** 字符串构建器对话框中输入以下表达式：

```
ln("count")
```

![](img/2e88bf5938bf48a409c0a534a8775b21_325_0.png)

