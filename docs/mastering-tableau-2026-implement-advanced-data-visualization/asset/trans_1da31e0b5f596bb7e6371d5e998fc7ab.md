

## 将数据导出到移动设备

开发仪表板时，Tableau Creator 提供了查看设备设计器或设备预览的选项。只要您处于仪表板选项卡中，就可以在此处找到：

![](img/66cf6b89349fda3581a4f196cb3c98a4_69_0.png)

图 1.50：设备预览

Tableau 自带默认手机设置。如有需要，可以点击 Phone，然后点击三个点来调整这些默认设置。进入设备设计器模式后，选择设备类型选项，您将获得最常见机型的选项。请注意，您只能使用仪表板默认布局中的工作表。例如，如果要为平板电脑添加默认布局，请进入设备设计器模式，选择 Tablet，并移动内容，使您希望在平板电脑上看到的工作表位于设备框架内。满意后？则将新布局（如图 1.50 所示）添加到工作簿中。它将出现在左上角区域的 Default 下方。用户现在可以在从 Tableau Server 打开工作簿时选择所需的设计。

不仅可以在移动设备上方便地查看您的项目，还可以使用 Tableau Mobile 随时随地处理它们！我们将在下一节中介绍这一点。

## Tableau Mobile

为支持灵活办公，Tableau 创建了一款可从 App Store 或 Google Play 下载的应用，名为 Tableau Mobile。将其安装在手机和平板电脑上后，您可以选择将应用连接到 Tableau Server 或 Tableau Online。请发布本章中“构建仪表板”部分创建的仪表板，发布到上述两个实例中的任意一个。如果您对发布有疑问，请参阅第 13 章 探索 Tableau Server 和 Tableau Cloud，以获取进一步说明。

在登陆页面底部，点击 **Explore** 以查看刚刚发布的所有视图。点击 **Building a Dashboard** 视图，查看之前创建的仪表板。最精彩的部分还在后面。仪表板完全交互，因此请尝试下拉筛选器或 **Medals** 筛选器。另外，尝试点击标记并选择 **Keep Only**。点击右上角的工作簿图标（从左数第一个图标），将显示您创建的所有其他工作表，从而可以在工作表之间切换：

![](img/66cf6b89349fda3581a4f196cb3c98a4_70_0.png)

图 1.51：工作表选择

现在，如果点击右上角的三个点，会出现更多选项，例如 **Revert**、**Alert**、**Subscribe** 等等。要了解 Tableau Server 上也可用的不同选项的更多信息，请参阅 _第 13 章 探索 Tableau Server 和 Tableau Cloud_。目前，我们可以得出结论：Tableau Mobile 让您无论身在何处都能轻松查看喜爱的仪表板。

## 使用基础知识的跟随式仪表板

为让您有另一个机会练习 Tableau 技能，我为您准备了以下练习。直接跟随操作即可！我们将使用来自 **Our World in Data (OWID)** 的 **Global CO2 Emissions by Country** 数据集。

此数据集非常适合用于练习 Tableau，因为它包含各国的年度 CO2 排放量、人口和人均值；非常适合学习比较、趋势和地理可视化：

- 1. 访问 https://github.com/owid/co2-data 处的 OWID CO2 数据集存储库并下载 CSV 文件。
- 2. 将文件保存在计算机上。我将其命名为 OWID.csv。
- 3. 打开 Tableau Desktop。
- 4. 在开始页面，单击 连接 | 到文件 | 文本文件。
- 5. 选择下载的 OWID.csv 文件：

![](img/66cf6b89349fda3581a4f196cb3c98a4_71_0.png)

![](img/66cf6b89349fda3581a4f196cb3c98a4_71_1.png)

![](img/66cf6b89349fda3581a4f196cb3c98a4_71_2.png)

需要添加数据方面的帮助？了解更多

![](img/66cf6b89349fda3581a4f196cb3c98a4_71_3.png)

图 1.52：OWID.csv

- 6. 打开新工作表，将 Year 拖到 Columns 容器，将 Co2 拖到 Rows 容器，并将工作表命名为 Emission Trends。
- 7. 若要仅包含未进行任何聚合的国家（例如所有经济合作与发展组织 (OECD) 国家），请将 ISO Code 移至 Filter 容器并取消选择 Null 值。再次点击筛选器，然后选择 应用到工作表 | 使用此数据源的所有工作表。

![](img/66cf6b89349fda3581a4f196cb3c98a4_72_0.png)

图 1.53：排放趋势

您现在已完成三个工作表中的第一个。折线图显示排放趋势。

- 8. 接下来，添加新工作表，并将 **Country** 拖到 **Rows** 容器。同时，将 **Co2** 拖到 **Columns** 容器。然后，将 **Year** 拖到 **Filter** 容器，并筛选最近一年（例如 2023）。确保您的 **Year** 字段为离散型（蓝色）。
- 9. 现在，将图表按降序排序，并为工作表指定标题 **Top Emitters: <Year>**。通过将 <Year> 变量添加到标题，标题将根据您选择的年份自动调整。**ISO Code** 筛选器继承自 **Emission Trends** 工作表，因为我们已将该筛选器应用于使用 OWID 数据的所有工作表：

![](img/66cf6b89349fda3581a4f196cb3c98a4_72_1.png)

图 1.54：排名

- 10. 为补充最后一部分，打开新工作表，并将 **Country** 拖到 **Marks** 卡的 **Detail** 中。Tableau 将其识别为地理字段，并创建世界地图。
- 11. 接下来，将 **Co2_per_capita** 拖到 **Color**，将 **Year** 拖到 **Filter** 容器，并将筛选器设置为与第二个工作表 **Top Emitters** 中使用的相同年份。
- 12. 调整颜色刻度以突出差异（例如，颜色越深 = 人均排放量越高）。我将此工作表命名为 World Emission: <Year>。

![](img/66cf6b89349fda3581a4f196cb3c98a4_73_0.png)

图 1.55：世界排放

要从三个工作表进入交互式仪表板，必须执行以下操作：

- 13. 通过单击页面底部的相应选项卡打开新仪表板。
- 14. 从左侧窗格中，将工作表（**Emission Trends**、**Top Emitters**、**World Emission**）拖到画布上。
- 15. 将它们以整洁的网格布局排列：**Emission Trends**（左上角）、**Top Emitters**（右上角）、**World Emission**（底部）：

### Emission Trends

### Top Emitters: 2023

![](img/66cf6b89349fda3581a4f196cb3c98a4_74_0.png)

### World Emission: 2023

![](img/66cf6b89349fda3581a4f196cb3c98a4_74_1.png)

图 1.56：仪表板

- 16. 添加标题，例如 Global CO2 Emissions Dashboard。
- 17. 点击仪表板上的 Top Emitters 工作表，然后选择 Filters | Year：

### Top Emitters: 2023

![](img/66cf6b89349fda3581a4f196cb3c98a4_74_2.png)

图 1.57：筛选器

- 18. 点击新创建的筛选器，然后选择 **Multiple Values (dropdown)**：

![](img/66cf6b89349fda3581a4f196cb3c98a4_75_0.png)

图 1.58：筛选器 2

- 19. 同样，在同一筛选器中点击 **Apply to Worksheets | Selected Worksheets**，并也选择 **World Emission** 工作表：

![](img/66cf6b89349fda3581a4f196cb3c98a4_75_1.png)

图 1.59：将筛选器应用到工作表

- 20. 要清理仪表板，请点击世界地图（右下角）上的 **1 unknown** 按钮，并选择 **Filter Data**。

![](img/66cf6b89349fda3581a4f196cb3c98a4_76_0.png)

图 1.60：应用筛选器

- 21. 最后，点击 World emission 工作表，然后选择右侧的筛选器图标，以将地图用作两个上方工作表的筛选器：

![](img/66cf6b89349fda3581a4f196cb3c98a4_76_1.png)

图 1.61：用作筛选器

结果是您自己的排放仪表板，显示一个折线图，用于比较 **国际标准化组织** (ISO) 国家随时间的变化排放量，以及一个条形图，显示最近一年 CO2 排放量最高的国家，并带有筛选选项以选择数据集中存在的任何其他年份。最后，您拥有一个突出显示人均排放量的世界地图，点击任何给定国家时，该地图也可用作筛选器。

此组合可全面了解全球排放并支持交互式探索。希望您喜欢！随意添加更多功能，并将其上传到您的 Tableau Public 个人资料。

## 摘要

在本章中，我们介绍了 Tableau 的基础知识。我们从一些基本术语开始，然后研究了创建工作表和仪表板的基础知识。我们关注默认行为以及如何覆盖该行为，并探讨了一些最佳实践。接着，我们回顾了 **Measure Names** 和 **Measure Values** 的基本原则。之后，我们探索了三个基本 Tableau 概念：维度和度量；行级、聚合级和表级计算；以及连续值和离散值的概念。特别是，需要理解行级和聚合级计算由数据源引擎计算，而表级计算由 Tableau 处理。最后，我们了解了如何为其他设备（如手机或平板电脑）调整仪表板，并观看了 Tableau Mobile 的实际应用。

在下一章中，我们将继续探索 Tableau，查看数据。我们将考虑如何使用 Tableau 的数据处理引擎 Hyper 准备数据，并探索一些有用的数据准备模型和技术。

## 获取本书的 PDF 版本和更多内容

扫描二维码（或访问 packtpub.com/unlock）。按名称搜索此书，确认版本，然后按照页面上的步骤操作。

![](img/66cf6b89349fda3581a4f196cb3c98a4_77_0.png)

**立即解锁**

*注意：请保管好您的发票。直接从 Packt 购买不需要发票。*

