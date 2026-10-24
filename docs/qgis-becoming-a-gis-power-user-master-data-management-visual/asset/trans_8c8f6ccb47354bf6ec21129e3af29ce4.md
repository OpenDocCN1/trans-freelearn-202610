

+   4. 如有必要，使用后退箭头选择 **State Legislative Districts**，然后点击 **submit**；为 **State Legislative Districts - Lower Chamber (current)** 选择 **Pennsylvania** 并点击 **Download**。
+   5. 将两个目录都移动到 `c5/data/original` 并解压它们。

> 我们仅为本次练习下载了一份边界数据集。由于边界每年并不一致，在实际项目中你需要下载并进一步处理每个单独的年度边界文件。

## Tabular data from American FactFinder

美国社区调查 FactFinder 网站上提供了许多不同的人口统计数据集。这些数据集与前面提到的 TIGER/Line 数据相辅相成，为 TIGER/Line 地理边界提供了属性数据。主要技巧是选择属性数据与地理边界数据之间匹配的地理边界层级和范围。请执行以下步骤：

+   1. 前往美国人口普查局 American FactFinder 网站 http://factfinder.census.gov。
+   2. 点击 **ADVANCED SEARCH** 选项卡。
+   3. 在 **topic or table name** 输入框中输入 `White` 并在建议选项中选择 **B02008: WHITE ALONE OR IN COMBINATION WITH ONE OR MORE RACES**。然后，点击 **GO**。
+   4. 从侧边栏中，在 **Geographies** 部分的 **Select a geographic type:** 下拉菜单中，选择 **Census Tract - 140**。
+   5. 在 **select a state** 下，选择 **Pennsylvania**；在 **Select a county** 下，选择 **Philadelphia**；在 **Select one or more geographic areas and click Add to Your Selections;** 下，选择 **All Census Tracts within Philadelphia County, Pennsylvania**。然后，点击 **ADD TO YOUR SELECTIONS**。
+   6. 从侧边栏进入 **Topics** 部分。在此，在 **Year** 下的 **Select Topics to add to 'Your Selections'** 中，点击从 **2009** 到 **2013** 的每个可用年份，将其逐一添加到 **Your Selections** 以便后续下载。

## Demonstrating Change

7. 检查 **Search Results** 选项卡下提供的五个数据集。所有已勾选的数据集都会被添加到待下载的选择中，如以下屏幕截图所示：

![](img/2e88bf5938bf48a409c0a534a8775b21_336_0.png)

8. 现在，从页面左上角显示选择的搜索筛选器中移除 **B02008: WHITE ALONE OR IN COMBINATION WITH ONE OR MORE OTHER RACES**。

9. 在 **topic or table name** 字段中输入 **total**，从建议的数据集中选择 **B01003: TOTAL POPULATION**，然后点击 **GO**。

10. 选择 2009 至 2013 年的总人口 5 年估计值，然后点击 GO。

![](img/2e88bf5938bf48a409c0a534a8775b21_337_0.png)

11. 点击 Download 下载这 10 个数据集，如前一屏幕截图所示。

12. 一旦看到 Your file is complete 消息，再次点击 DOWNLOAD 下载文件。这些文件将作为 aff_download.zip 目录下载。

13. 将此目录移动到 c5/data/original 然后解压它。

## Preparing and exporting the data

首先，我们将介绍表格数据准备和导出的步骤，这与我们之前做过的步骤相当相似。接下来，我们将介绍边界数据准备的步骤，这将更为新颖。我们需要基于图层之间的空间关系来准备此数据，这需要使用 SQLite，因为这无法通过 QGIS 的开箱即用或插件功能轻松完成。

## The tabular data

我们的表格数据是人口普查区白人人口。我们只需要在此数据中具有可解析的经纬度字段以便稍后绘图，因此可以将其保留在此通用表格格式中。

## Combining it yearly

要将此年度数据合并，我们可以在 QGIS 中基于共同的 GEO ID 字段连接每个表。请执行以下步骤：

+   1. 打开 QGIS 并导入所有边界 shapefile（普查区和州众议院边界）以及数据表（所有下载的人口普查区年份）。单个边界 shapefile 将在其解压目录中，扩展名为 .shp。数据表将命名为类似 x_with_ann.csv 的格式。你需要以与之前相同的方式添加它们，即通过图层菜单下的 Add Vector Layer。以下是要添加的所有文件列表：
+      - t1_2014_42_tract.shp
+      - ACS_09_5YR B01003_with_ann.csv
+      - ACS_10_5YR B01003_with_ann.csv
+      - ACS_11_5YR B01003_with_ann.csv
+      - ACS_12_5YR B01003_with_ann.csv
+      - ACS_13_5YR B01003_with_ann.csv
+   2. 从图层面板中选择普查区边界 shapefile，t1_2014_42_tract。
+   3. 导航到 Layers | Properties。
+   4. 对于每个白人人口数据表（以 x_B02008_with_ann 结尾），执行以下步骤：
+      1. 在 Joins 选项卡上，点击绿色加号 (+) 添加连接。
+      2. 选择数据表作为 Join layer。
+      3. 在 Join field 选项卡中选择 GEO.id2。
+      4. Target field: GEO ID

![](img/2e88bf5938bf48a409c0a534a8775b21_339_0.png)

连接所有表后，你会在属性表中发现许多包含空值的行。如果对它们按年份排序，你会发现我们为较近年份填充的行数与费城普查区图层中的行数相同。然而，2009 年（ACS_09_5YR_B01003_with_ann.csv）有许多行无法填充，这是由于 2014 边界数据中使用的唯一标识符发生了变化所致。因此，我们将从分析中排除 2009 年。你可以从已连接的表中移除 2009 年数据表，这样以后就不会出现问题。

## Demonstrating Change

现在，将已连接的图层导出为新的 DBF 数据库文件，我们需要这样做以便进行最后的更改：

+   1. 确保仅在普查区图层中选择了具有填充数据列的行。归因于该表（你可以通过在该字段上对属性表进行排序来完成，例如）。
+   2. 从 **Layers** 面板中选择普查区图层。
+   3. 导航到 **Layer | Save as**，满足以下参数：
+      - **Format: DBF File**
+      - Save only the selected features
+      - Add the saved file to the map
+      - **Save as:** `c5/data/output/whites.dbf`
+      - 将其他选项保持为默认设置

## Updating and removing fields

QGIS 允许我们计算地理要素的坐标并将其填充到属性字段中。在新 DBF 图层的图层上，按预期格式计算经纬度字段并通过执行以下步骤消除不必要的字段：

+   1. 打开 whites DBF 图层的 **Attribute table** 并点击 **Open Field Calculator** 按钮。
+   2. 计算新的 lon 字段并满足以下参数：
+      - **Output field name:** `lon`
+      - **Output field type:** Decimal number (real).
+      - **Output field width:** 10.
+      - **Precision:** 7.
+      - **Expression:** `"INTPLON"`. 你可以从 **Functions** 面板下 **Fields and Values** 部分的树中选择此项。

![](img/2e88bf5938bf48a409c0a534a8775b21_341_0.png)

3. 重复这些步骤处理纬度，从 INTPLAT 创建 lat 字段。

4. 使用字段计算器和右侧的表达式创建以下字段：

+   - Output field name: name; Output field type: Text; Output field width: 50; Expression: NAMESLAD
+   - Output field name: Jan-11; Output field type: Whole number (integer); Expression: "ACS_11_5_2" - "ACS_10_5_2"
+   - Output field name: Jan-12; Output field type: Whole number (integer); Expression: "ACS_12_5_2" - "ACS_11_5_2"
+   - Output field name: Jan-13; Output field type: Whole number (integer); Expression: "ACS_13_5_2" - "ACS_12_5_2"

## Demonstrating Change

+   5. 移除所有旧字段（除 name、Jan-11、Jan-12、Jan-13、lat 和 lon 外）。这将从表中移除所有不必要的标识字段和误差范围字段。

+   6. 切换编辑模式并在提示时保存。

![](img/2e88bf5938bf48a409c0a534a8775b21_342_0.png)

