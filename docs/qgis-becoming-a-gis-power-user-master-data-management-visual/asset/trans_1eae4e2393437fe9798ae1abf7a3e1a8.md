
4. 点击**确定**按钮保存更改并关闭对话框。现在，我们的数据将正确显示：

![](img/2e88bf5938bf48a409c0a534a8775b21_742_0.png)

另一种选择是将数据裁剪到日期变更线。这可以使用 GDAL 工具集中的 `ogr2ogr` 工具完成。为此，请执行以下步骤：

- 1. 如果您是 Windows 用户，请打开 OSGeo4W 命令提示符；如果您是 Linux 或 Mac OS 用户，请打开终端窗口。
- 2. 将目录更改为 `honolulu-flights.shp` 所在的文件夹，例如以下目录：

  `cd c:\data`

- 3. 在命令提示符中输入以下命令。注意，我们先指定输出文件，再指定输入文件：

  `ogr2ogr -wrapdateline honolulu-flights-wrapped.shp honolulu-flights.shp`

4. 将新创建的 `honolulu-flights-wrapped.shp` 图层加载到 QGIS 中后，您将看到包裹在日期变更线上的航班正确显示：

![](img/2e88bf5938bf48a409c0a534a8775b21_743_0.png)

5. 现在从 `new-zealand.sqlite` 数据库加载 `nz-coastlines-and-islands` 图层。您应该会看到两组相距很远的多边形。这就是新西兰岛和查塔姆群岛，它们本应位于附近。

![](img/2e88bf5938bf48a409c0a534a8775b21_743_1.png)

为了正确显示它们，我们可以使用 SpatiaLite 中提供的 `ST_Shift_Longitude` 函数。为此，请执行以下步骤：

- 1. 通过单击工具栏上的按钮或导航到**数据库 | DB Manager | DB Manager**，打开 **DB Manager** 插件。
- 2. 在对话框左侧的连接树中展开 **SpatiaLite** 组，然后选择 `new-zealand` 数据库。
- 3. 打开 **SQL 窗口**并运行以下查询：

```
UPDATE "nz-coastlines-and-islands" SET
Geometry=ST_Shift_Longitude(Geometry)
```

4. 从 QGIS 中移除 `nz-coastlines-and-islands` 图层并重新添加。现在，新西兰岛和查塔姆群岛将正确显示，如下所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_744_0.png)

值得一提的是，`ST_Shift_Longitude` 函数在 PostGIS 中也可用。

## 工作原理

当我们启用“即时”CRS 变换时，每个几何图形都会被重新投影到给定的 CRS（在我们的示例中是以太平洋为中心），因此坐标现在具有相同的符号。

带有 `-wrapdateline` 选项的 `ogr2ogr` 工具会将所有跨越日期变更线的几何图形分割并写入输出文件。不跨越日期变更线的几何图形将原样复制。

`ST_Shift_Longitude` 函数将负经度平移 360 度，因此所有数据将位于 0-360 度范围内，并正确显示。

## 另请参阅

- http://docs.qgis.org/2.2/en/docs/user_manual/working_with_vector/supported_data.html#vector-layers-crossing-180-degrees-longitude

## 离线工作

互联网是一种极好的资源，但有时你就是无法访问。对于野外作业、服务不稳定的地区、飞机上，甚至是会议室里，你也许无法获得所需的所有资料。这里的资料指的是文档（用户文档和开发文档），但更重要的是数据库图层（例如 PostGIS）和基于 Web 服务的图层（例如 WMS、WFS、OpenLayers 插件等）。

本方案介绍如何在前往无网络连接的地方之前，将所需文件的本地副本缓存到计算机上。

### 准备工作

对于本方案，您需要打开一个 PostGIS 数据库或 WFS，并启用 QGIS 附带的**离线编辑**插件。

### 操作方法

- 1. 从 PostGIS 或 WFS 加载您的图层。
- 2. 确保激活**离线编辑**选项。
- 3. 在**数据库**菜单中，您应该会看到**离线编辑**，选择**转换为离线项目**：

![](img/2e88bf5938bf48a409c0a534a8775b21_745_0.png)

- 4. 选择用于存储数据的本地文件。
- 5. 然后，选择要转换的所有图层（仅限矢量图层）：

![](img/2e88bf5938bf48a409c0a534a8775b21_745_1.png)

- 6. 现在，您可以在不连接在线数据库的情况下使用您的 PostGIS 图层。请尝试进行一些编辑。（我们建议您在数据库或表的一个副本上执行此操作，直到您完全熟悉为止。）
- 7. 当您重新连接到网络时，您可以通过选择**同步**选项将更改发送到数据库：

![](img/2e88bf5938bf48a409c0a534a8775b21_746_0.png)

## 工作原理

基本原理很简单：您数据的副本会保存在您计算机本地的 SpatiaLite 数据库中。项目文件会记录更改，一切就绪，因为 SpatiaLite 可以执行 QGIS 中任何其他矢量数据源可以执行的任何操作。

在多用户环境中工作时请务必小心，如果多个用户独立修改同一数据集，此方法不处理编辑冲突。

## 更多内容

此外，还有多种方法可以为栅格数据集（网络文件或 Web 服务）创建离线缓存，包括 `gdalwmscaching` 或 `mbtiles`。如果您计划有一段时间需要在没有互联网的情况下工作，请提前规划，并在真正需要离线之前测试解决方案。再多的插件也无法替代良好的规划。

请记住，在缓存 Web 服务（例如 Google 和 Bing）之前，请检查其合法性。OpenStreetMap 是离线使用的可靠瓦片来源，且不受限制。

## 另请参阅

- GDAL WMS 缓存选项可在 http://www.gdal.org/frmt_wms.html 获取
- 关于 mbtiles 用法的讨论可在 http://blogs.terrorware.com/geoff/2012/11/17/offline-map-tiles-in-qgis/ 获取
- 一个可帮助您进行设置的新插件可在 https://plugins.qgis.org/plugins/MBTiles2img/ 获取

## 使用 QspatiaLite 插件

有时，您可能不需要将整个图层加载到 QGIS 中，而只需要其中的某个子集，或者动态执行一些计算。在这种情况下，运行复杂 SQL 查询并在 QGIS 中显示其结果的能力将非常有用。

本方案介绍如何使用 QspatiaLite 插件执行 SQL 代码并在 QGIS 中加载数据。

### 准备工作

要遵循本方案，您需要创建到 cookbook.db 数据库的连接，该数据库是在第 1 章“数据输入与输出”中的“将矢量图层加载到 SpatiaLite”方案中创建的。或者，您可以使用自己的 SpatiaLite 数据库，但请注意，您可能需要修改下面的一些 SQL 语句以匹配您的表。

此外，从插件管理器安装 QspatiaLite 插件。

### 操作方法

确保您已创建到 cookbook.db 的连接。通过导航到 数据库 | SpatiaLite | QspatiaLite 启动 QspatiaLite 插件：

![](img/2e88bf5938bf48a409c0a534a8775b21_747_0.png)

要执行 SQL 查询，请执行以下步骤：

- 1. 从插件对话框左上角的组合框中选择要使用的数据库。
- 2. 在 **SQL** 选项卡中，输入以下查询：

```
SELECT "census_wake2000".'pk' AS id,
"census_wake2000".'geom' AS Geometry,
"census_wake2000".'area', "census_wake2000".'perimeter'
FROM "census_wake2000" WHERE "census_wake2000".'perimeter' > 100000;
```

在对话框左侧的**表**树中双击表名和列名，即可轻松插入它们。

- 3. 单击对话框底部的**运行**按钮执行查询。**结果**选项卡将自动打开，您可以以表格形式查看查询结果，如下所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_748_0.png)

如果查询结果包含几何信息（即所谓的几何列），您可以在 QGIS 中显示它们。为此，请执行以下步骤：

- 1. 切换回 **SQL** 选项卡。
- 2. 在**选项**组合框中，选择要执行的操作，例如**创建空间视图并加载到 QGIS**。
- 3. 在**表**字段中，输入结果视图的名称，例如 `above100k`。

