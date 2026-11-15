
当我们在 Tableau 中谈论 `Level of Detail` (`LOD`) 计算时，指的是三种表达式：`FIXED`、`INCLUDE` 和 `EXCLUDE`。这三种表达式通过提供创建以特定粒度级别为目标的计算的能力，开启了一个全新的选项世界。在旧版本的 Tableau 中，工作表的数据粒度由视图中的维度决定。如果视图包含例如 `Region`、`State` 和 `Postal Code` 等维度，但作者想要创建一个 `City` 级别的计算，则必须在视图中包含 `City` 维度。此外，没有机制在视图上排除或忽略给定的 `dimension`。诚然，通常可以通过一些复杂且有时曲折的表计算、数据混合等用法来获得期望的结果。幸运的是，LOD 极大地简化了这些用例场景，并在某些情况下实现了以前不可能的功能。

在本章中，我们将讨论以下主题：

- Introducing LODs（LOD 简介）
- `FIXED` 和 `EXCLUDE`
- `INCLUDE`
- Building practical applications with LODs（使用 LOD 构建实用应用）

本章前几节更多地基于理论，但请别担心——您将在 *Building practical applications with LODs* 一节中以实用的方式探索这些概念。让我们从介绍 LOD 及其用法开始。

# Introducing LODs（LOD 简介）

Tableau 的默认行为是根据视图中也存在的维度来显示度量。如果您有一个包含 `Sales` 数据和 `State`、`City` 等维度的仪表板，并将 `State` 和 `Sales` 数据拖到视图上，`Sales` 数据将按 `State` 划分，显示每个 `State` 的 `Sales`。如果您想将 `Sales` 数据进一步细分为更小的块，可以添加 `City` 字段，从而得到按 `State` 划分的每个 `City` 的 `Sales` 数据。LOD 可以操控这种默认行为。

完成本章后，您将能够按视图中不存在的维度来划分或分区度量，并使用比视图中可见维度更少的维度来显示度量。

为此，我们将构建并使用两个 `playgrounds`（演练环境）。根据工作职责交付报告可能会导致对有限功能集的透彻了解；即理解深但面窄。很难抽出时间（并证明该时间合理）去探索 Tableau 那些表面上似乎与工作职责没有直接关联的功能。演练环境可以通过提供高效的探索途径来帮助克服这些困难和异议。在本章中，我们将专门为 LOD 构建两个演练环境，通过提供高效的探索和理解途径，使深度和广度理解的任务变得更容易。您随时可以回到随本章附带的工作簿，测试与 LOD 相关的功能。

# FIXED and EXCLUDE（FIXED 和 EXCLUDE）

我们将构建的第一个演练环境旨在探索三个 LOD 函数中的两个：`FIXED` 和 `EXCLUDE`。我们将使用一组参数和相关的计算字段来高效地探索这些函数的工作原理。

## Setting up the workbook（设置工作簿）

本练习的大部分基础工作已在随本章附带的工作簿中完成。以下步骤只需您打开不同的计算和参数，查看它们是如何设置的以及为什么这样设置。沿途会给出解释。如果您无法直接访问该工作簿，应能通过参考以下步骤构建一个类似的工作簿。

要完成工作表的初始设置，请执行以下步骤：

1. 导航至 [Tableau Public 上的 Marleen Meier 个人资料](https://public.tableau.com/profile/marleen.meier) 以查找并下载随本章附带的工作簿。
2. 打开随本章附带的工作簿并导航至 `Fixed and Exclude` 工作表。工作表应如下所示：
   ![](Images/B18435_07_01.png)
   图 7.1：工作表
3. 选择 `Superstore` 数据源，通过右键单击并选择 `Edit` 来检查名为 `1st Dim` 的参数：
   ![](Images/B18435_07_02.png)
   图 7.2：参数 I
4. 名为 `2nd Dim`、`3rd Dim` 和 `4th Dim` 的参数与 `1st Dim` 相同。
5. 请注意，除 `Order Date` 和 `Ship Date` 外，数据集中的每个维度都包含在值列表中（请参阅 *图 7.2*）。就本练习而言，`Category`、`Region`、`Segment`、`Ship Mode` 和 `Sub-Category` 尤为重要，因为这些是成员较少的维度。成员较多的维度在此上下文中更难使用。
6. 检查工作簿中包含的 `Choose Fixed Dims`、`Choose Excluded Dims 1` 和 `Choose Excluded Dims 2` 参数，请注意我们在 `Value` 列下定义了几个整数，每个整数稍后将在参数下拉列表的 `Displays As` 列下指定的字符串形式显示。`Choose Fixed Dims` 的 `Value` 和 `Display As` 配置如下：
   ![](Images/B18435_07_03.png)
   图 7.3：参数 II
7. 检查名为 `1st Dim` 的计算字段（它位于 `Data` 窗格中的 `Practical: All` 文件夹下）；此计算使用了 `1st Dim` 参数，并将根据参数的选择显示一个字段。如果用户在 `1st Dim` 参数中选择 `Category`，则值 `1` 将被激活（根据 *图 7.2* 中指定的值）。值 `1` 随后会在计算的 `1st Dim` 字段中转换为 `Category` 字段，如下截图所示：
   ![](Images/B18435_07_04.png)
   图 7.4：计算的 1st Dim 字段
8. 名为 `2nd Dim`、`3rd Dim` 和 `4th Dim` 的计算字段与 `1st Dim` 相同，只是每个字段引用其同名参数；例如，`2nd Dim` 计算字段使用 `[Parameters].[2nd Dim]`。
9. 这些 `Case` 语句结合相关参数，将允许您从视图中的下拉列表中选择要查看的维度，如 *步骤 5* 所述。
10. 检查以下计算字段，它们使您能够选择要采用的 LOD，从而比较和对比差异与相似之处：
    ![](Images/B18435_07_05.png)
    图 7.5：Case Fixed
    `Case Fixed` 计算显示，如果用户在 `Choose Fixed Dims` 参数中选择 `1`（显示为 `1st`），则 `1st Dim` 将被固定——无论仪表板设置如何。这意味着 `SUM(Sales)` 将按 `1st Dim` 分区，无论它是否属于仪表板的一部分：
    ![](Images/B18435_07_06.png)
    图 7.6：Case Exclude 1
    `Case Exclude 1` 计算显示，如果用户在 `Choose Excluded Dims 1` 参数中选择 `1`（将显示为 `1st`），则 `1st Dim` 将从视图中的计算中排除。这意味着 `SUM(Sales)` 不会按 `1st Dim` 分区，即使它是视图的一部分：
    ![](Images/B18435_07_07.png)
    图 7.7：Case Exclude 2
    `Case Exclude 2` 计算显示，如果用户在 `Choose Excluded Dims 2` 参数中选择 `1`（将显示为 `1st`），则 `1st Dim` 将从视图中的计算中排除。这意味着 `SUM(Sales)` 不会按 `1st Dim` 分区，即使它是视图的一部分。
11. 现在，右键单击并打开 `Product Name` 上的上下文筛选器以进行检查。在生成的对话框中，单击 `Wildcard` 选项卡并注意设置，如下截图所示。筛选器已设置为仅显示以 `S` 开头的产品名称（`Match value` 不区分大小写）：
    ![](Images/B18435_07_08.png)
