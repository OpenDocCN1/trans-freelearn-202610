

在 Revit Autodesk 应用商店中，我们可以找到一个按特定功能组织插件的菜单：

![](img/1961add582dd4a589b2eac3dda140e38_422_0.png)

*图 9.2 – Revit Autodesk 应用商店菜单*

我们也可以通过点击 Revit 屏幕右上角的图标来访问 Revit Autodesk 应用商店：

![](img/1961add582dd4a589b2eac3dda140e38_422_1.png)

*图 9.3 – 访问 Revit Autodesk 应用商店的图标*

在 Revit Autodesk 应用商店中，我们可以找到免费和付费插件。

在安装插件之前，请与 IT 部门和您的技术负责人确认该插件是否适合您的工作流程，以及您是否可以下载或请求将其安装到您的系统上。

在将项目从一个版本升级到另一个版本之前，我们也需要检查以确保使用的所有插件在新版本中都已可用，以避免对当前工作流程产生任何影响。

您还需要检查插件是否与您当前使用的 Revit 版本兼容，并查看其是否最近已更新。点击 Revit 插件即可找到有关发布日期、上次更新日期和兼容 Revit 版本的信息：

![](img/1961add582dd4a589b2eac3dda140e38_423_0.png)

*图 9.4 – 有关 Revit 插件的信息*

> **提示**
> 建议在安装插件之前关闭 Revit。

当我们安装插件后打开 Revit 时，会出现以下消息。必须选择 **始终加载** 选项以避免出现问题：

![](img/1961add582dd4a589b2eac3dda140e38_424_0.png)

*图 9.5 – 始终加载 Revit 插件的选项*

如果我们选择 **不加载**，将会产生两个问题：

- 该应用仍会安装在您的 PC 上，占用硬盘上的宝贵空间。
- 如果我们决定使用该插件，则首先需要卸载然后重新安装该插件。

## 让我们学习有关安全性的知识！

就安全性而言，我们应优先考虑使用微软提供的证书签名且来自受信任发布者的应用。

在下图中，我们可以看到未签名和已签名插件之间的比较。我们可以识别插件是否已使用证书签名。

启动 Revit 时，安装后会出现以下窗口。请参考指向差异的箭头：

![](img/1961add582dd4a589b2eac3dda140e38_425_0.png)

*图 9.6 – 未签名和已签名插件*

如果插件未签名，我们应检查开发商是否是一家受信任的公司。

## 删除 Revit 插件

插件会减慢 Revit 的启动速度，但这种减慢是微不足道的，因此我们无需担心。

另一方面，插件可能会与其他插件冲突或影响 Revit 中的特定功能，从而减慢速度并可能导致项目崩溃。

如果您遇到问题，可以从 Windows | 控制面板 中删除插件并在没有该插件的情况下测试 Revit：

![](img/1961add582dd4a589b2eac3dda140e38_426_0.png)

*图 9.7 – Windows 控制面板中删除插件的选项*

如果插件删除不成功，或插件未出现在待删除列表中，我们需要手动卸载插件。

手动删除插件可能具有挑战性，因为您需要在 C:\ 硬盘的多个位置找到它们。

我们可以在以下路径找到已安装的 Revit 插件：

- C:\ProgramData\Autodesk\Revit\Addins\
- C:\Users\%Username%\AppData\Roaming\Autodesk\Revit\Addins\
- C:\ProgramData\Autodesk\ApplicationPlugins
- C:\Program Files\Autodesk\Revit 2021\AddIns

> **注意**
> 您可能需要根据 Revit 版本选择文件夹。

一旦找到所需的插件，我们可以选择并删除文件以从 Revit 中移除。

如果您希望在没有插件的情况下测试 Revit，可以将插件文件移动到另一个文件夹并重新启动 Revit。测试后，您可以将插件移回同一文件夹。

> **提示**
> 使用 Stantec 提供的免费插件来禁用 Revit 插件。

Stantec 提供了一个 *很棒的、免费的应用*，可快速启用和禁用 Revit 插件。

我们可以在以下链接找到下载和说明：

```
https://bit.ly/3qDb9Ii
```

二维码如下：

![](img/1961add582dd4a589b2eac3dda140e38_427_0.png)

该应用将允许您选择多个插件以启用或禁用它们：

![](img/1961add582dd4a589b2eac3dda140e38_427_1.png)

*图 9.8 – Stantec 免费应用以启用和禁用插件*

## 创建 Revit 插件

Autodesk 允许用户创建 Revit 插件。如果您的组织中有精通 C# 和 VB.NET 编程的人员，他们可以访问 Revit 应用程序编程接口 (API) 来根据您的需求创建 Revit 插件。

我们可以在网上找到多个资源，但以下链接是 Autodesk 关于如何创建第一个 Revit 插件的教程：

```
https://autode.sk/3aqYcvC
```

二维码如下：

![](img/1961add582dd4a589b2eac3dda140e38_428_0.png)

总之，我们已经学习了管理 Revit 插件的流程。在下一节中，我们将学习提高生产力、可视化和分析的最佳 Revit 插件。

## Revit 插件

Autodesk 应用商店中有许多 Revit 插件，我们将重点介绍一些最佳插件，以提高在 Revit 中工作时的生产力并创建新的工作流程。

我们将插件分为两个类别：

- 生产力
- 可视化

## 生产力插件

我们将对每个插件进行简短描述，以解释它如何使您在 Revit 中的日常活动受益。插件来自 Autodesk 应用商店和开发商。我们建议您下载插件进行测试和进一步学习：

- **DiRoots 插件：** DiRoots 提供了一系列优秀的免费 Revit 插件，并且可以根据您的需求创建自定义插件。*ProSheets* 插件可以按正确的命名约定批量打印 PDF 文件。

  插件链接如下： https://bit.ly/3pB88Xp：

  ![](img/1961add582dd4a589b2eac3dda140e38_429_0.png)

  ProSheets

  免费 🎁 新更新

  轻松将视图/图纸导出为 PDF、DWG、DGN、DWF、NWC、IFC 和 IMG（JPEG、PNG、TIFF 等）。

  *图 9.9 – 来自 DiRoots 的 Revit 插件*

- **RTV Tools**：RTV Tools 有一个名为 *RTV Xporter* 的插件，可按正确的命名约定批量打印 PDF 文件。此工具在使用 Adobe Acrobat DC 和 PDF Creator 的 PDF 驱动程序时效果更好。

  插件链接如下： https://bit.ly/3dsI1jb。

- **Xrev Transmit**：Xrev Transmit 可以同时打印和导出多种格式，包括 DWF、DWG、DGN、DXF 和 PDF。

  插件链接如下： https://bit.ly/2MbCFxB。

- **Bonus Tools**：Bonus Tools 是同一产品中超过 140 个 Revit 插件的集合，对于在 Revit 中节省日常活动时间非常有用。我们可以复制带视图的图纸、列出就地族、在发布文件前删除视图和图纸等等。

  产品链接如下： https://bit.ly/2M7kU2g。

  各插件描述链接如下： https://bit.ly/3brnrwL。

- **pyRevit**：pyRevit 是一个出色的 Revit 插件集合，我们可以在其中制作图案、创建多张图纸、管理注释、跨项目复制图纸并使用许多其他功能。它还使您能够创建 Revit 插件并在组织内共享它们。

  下载链接如下： https://bit.ly/3bo6vYe。

  核心插件的 YouTube 链接如下： https://bit.ly/2NsLd3z。

- **RushForth Tools：** RushForth Tools 是项目设置、控制参数、将 Excel 表导入 Revit 等功能的插件集合。

  插件链接如下： `https://bit.ly/2NMWOdy`。

- **AGACAD：** AGACAD 提供多个免费和付费插件，以提高预制设计、木材和金属建筑设计的生产力。

  BIM 解决方案链接如下： `https://bit.ly/2NQE3pW`。

  Revit 工具链接如下： `https://bit.ly/37C1HgR`。

- **Ideate BIMLink：** Ideate BIMLink 使 Revit 用户能够以速度和准确性从 BIM 模型导出和导入大量数据到 Excel。Ideate 网站还提供其他插件以提高生产力。

  网站链接如下： `https://bit.ly/2MdsZCI`。

- **ArchSmarter Toolbox：** ArchSmarter Toolbox 是一个免费的 Revit 插件，具有多种功能，例如删除视图、对齐视图、替换字体、批量重命名和制作图纸。

  插件链接如下： `https://bit.ly/2M9SXXE`。

- **COINS Auto-Section Box：** COINS Auto-Section Box 工具使您能够在所选元素周围创建 Revit 剖面框，并并排比较平面和 3D 视图。它是一款免费工具，在设计 MEP 服务时提供出色的流程。

  工具链接如下： `https://bit.ly/3ujG66w`。

- **CAD Link Opener：** CAD Link Opener 是一个免费插件，可在从 Revit 打开 DWG 链接时节省时间。用户选择 DWG 链接，它将使用 AutoCAD 打开。

  插件链接如下： `https://bit.ly/3bvf4AF`。

- **Isolate Warnings：** Isolate Warnings 是一个免费插件，用于创建 3D 视图以隔离并突出显示具有警告的模型元素。

  工具链接如下： `https://bit.ly/3k9W3Y7`。

- **Layers Manager：** Layers Manager 是一个免费插件，可自动创建系统族的共享参数，我们可以使用它来标记并显示有关材料和厚度的信息。

  插件链接如下： `https://bit.ly/2NhCxgB`。

  如何使用 Layers Manager 的链接如下： `https://bit.ly/2ZyqnSU`。

