

## 设置工作区

在了解更多关于 **Base Flanges** 之前，让我们先确保工作区已正确设置用于钣金建模。钣金工具可通过顶部菜单栏上的 **Insert** 菜单找到（图 1.3），但使用 **Sheet Metal** 选项卡或工具栏更快更方便：

![](img/4e9c736de6bac4449096a7b92c28af4c_24_0.png)

图 1.3：Insert 菜单中的钣金工具

## 设置 Sheet Metal 选项卡或工具栏

要添加 Sheet Metal 选项卡，请按以下步骤操作：

- 1. 在 SolidWorks 零件文档中，右键单击命令管理器 Command Manager 上任何现有选项卡，或空白区域（图 1.4 中标记为 i）。
- 2. 选择 Tabs 子选项（图 1.4 中标记为 ii），并确保 Sheet Metal 选项旁有复选标记（图 1.4 中标记为 iii）。

![](img/4e9c736de6bac4449096a7b92c28af4c_25_0.png)

图 1.4：将 Sheet Metal 选项卡添加到命令管理器

这样就会将 Sheet Metal 选项卡添加到命令管理器的现有选项卡中。

要添加 **Sheet Metal** 工具栏：

- 1. **Sheet Metal** 工具栏可以像选项卡一样添加。首先，右键单击命令管理器 **Command Manager** 上任何现有选项卡，或空白区域（图 1.4 中标记为 i）。
- 2. 选择 **Toolbars** 子选项并确保 **Sheet Metal** 选项旁有复选标记。

> **应使用 Sheet Metal 工具栏还是 Sheet Metal 选项卡？**
> 默认情况下，**Sheet Metal** 选项卡和工具栏包含相同的一组工具。两者都可以在本书中使用，这取决于用户偏好。
> **Sheet Metal** 选项卡包含更大、更清晰的图标，但工具栏可以移动到 **Graphics Area** 以更方便地访问工具。

## 创建 Base Flange

**Base Flange** 是钣金零件的第一个特征，也是创建新钣金零件最简单的方式，同时也提供最大的灵活性。这是因为一旦创建了 **Base Flange**，就可以添加弯曲和凸缘等附加特征，以类似标准实体建模的方式构建 3D 零件。相比之下，将现有零件转换为钣金（本书后面会介绍）可能会限制可以创建的零件类型，并在后期减少编辑选项。

要实际创建 **Base Flange**，首先需要绘制一个草图轮廓。可使用各种轮廓类型，但本节将介绍最简单的一种：**Single Closed** 轮廓。

要创建 **Base Flange**：

- 1. 开始一个新的 SolidWorks 零件。
- 2. 在适当的平面上开始草图，例如 **Top Plane**（图 1.5 中标记为 i）。
- 3. 绘制 **Base Flange** 轮廓。稍后本章将介绍不同的轮廓类型，但现在，只需创建一个封闭轮廓，例如 **Center Rectangle**（图 1.5 中标记为 ii）。
- 4. 通过使用 **Smart Dimension** 工具并将其链接到 **Origin**（图 1.5 中标记为 iii）来完全定义轮廓。使矩形高度为 200mm，宽度为 300mm。

## 完全定义草图

在 SolidWorks 建模期间，完全定义草图非常重要，以确保草图实体固定且不会以意外的方式行为，例如移动位置。完全定义的草图实体将变为黑色，并且当草图完全定义时，屏幕右下角会显示提示（图 1.5 中标记为 iv）。

![](img/4e9c736de6bac4449096a7b92c28af4c_27_0.png)

图 1.5：绘制 Base Flange 轮廓

- 5. 退出草图（图 1.5 中标记为 v）。
- 6. 选择 Sheet Metal 选项卡或工具栏并选择 Base Flange/Tab 选项——这应该是第一个图标（图 1.6 中标记为 i）。
- 7. 系统会提示您选择一个草图。选择刚刚绘制的草图，应会出现 Base Flange 的黄色预览（见图 1.6）。

# Note

也可以跳过步骤 5，直接在草图内从 Sheet Metal 选项卡选择 Base Flange/Tab 选项。在这种情况下，正在编辑的草图将自动使用。

8. 输入以下设置（这些将在本章后面详细介绍）：

- Thickness: 1mm
- Bend Allowance: K-Factor, 0.5
- Auto Relief: Rectangular
- Ratio:0.5（图 1.6 中标记为 iii）

按 OK（图 1.6 中标记为 iv），Base Flange 特征即创建完成。

![](img/4e9c736de6bac4449096a7b92c28af4c_28_0.png)

图 1.6：创建 Base Flange 特征

Base Flange 特征现已创建，FeatureManager 设计树左侧会出现 Base-Flange1（注意：您的编号可能不同）（图 1.7 中标记为 i）。可以通过左键单击该特征并选择 Edit Feature 以通常的方式编辑和调整该特征。

创建 Base Flange 特征会将整个零件转变为钣金零件，因此会自动在 FeatureManager 设计树中添加一些额外项目。这些项目表示零件是钣金模型，由以下部分组成：

- Sheet-Metal 文件夹：包含钣金零件的所有全局属性，例如 Thickness 和 Bend Radius 属性（图 1.7 中标记为 i）。

- Flat-Pattern 文件夹：此文件夹包含零件的展开版本（图 1.7 中标记为 iii）。默认情况下，此文件夹将被抑制，因此在此阶段会显示为灰色，但这将在本章后面详细介绍。
- Cut list 文件夹：此文件夹包含构成零件的所有单独板材。可以将其视为与普通实体建模中使用的 Solid Bodies 文件夹类似

文件夹名称后的数字表示零件由多少张板组成。例如，由两个单独的本体和盖板组成的零件将在 Cut list 文件夹后显示 (2)（图 1.7 中标记为 iv）。

![](img/4e9c736de6bac4449096a7b92c28af4c_29_0.png)

图 1.7：钣金零件详情

Base Flanges 通常是开始钣金零件的最佳方式，并且通常是钣金零件中创建的第一个特征。它们可以通过绘制封闭轮廓，然后从钣金工具中选择 Base Flange/Tab 选项来制作。

添加 Base Flange 特征会将零件转变为钣金零件，并会自动添加 Flat-Pattern、Cut list 和 Sheet-Metal 文件夹，我们将在下一节中进一步探索这些文件夹。

> **保存零件**
> 在此阶段，您的当前 *part* 可以保存，因为我们将在整本书中使用此文档。
> 
> 在 SolidWorks 中工作时，养成定期保存工作的习惯，以防止崩溃和其他问题。

## 钣金属性

**Sheet Metal** 属性是适用于整个 *part* 的全局设置。本节介绍如何调整这些属性以及它们的实际含义。该部分包含一些相当枯燥的背景理论，但它是理解钣金的重要基础。其中一些底层理论在我们稍后实际使用和演示工具时将会变得更加清晰，因此如果某些部分在此阶段仍然有点令人困惑，请不要担心太多。

共有四个钣金属性：

- 1. **Sheet Thickness**
- 2. **Bend Radius**
- 3. **Bend Allowance**
- 4. **Auto Relief**

## 编辑钣金属性并调整板厚

如前所述，钣金 *parts* 通常由厚度恒定的板材制成。因此，*part* 的 **thickness** 是钣金的重要方面。

要编辑 *part* 的 **Sheet Metal** 属性，只需在 FeatureManager 设计树中左键或右键单击 Sheet-Metal 文件夹（图 1.7 中标记为 **ii**）并选择 **Edit Feature**。

要设置厚度，调整 **Thickness** 字段下的值（图 1.8 中标记为 **i**）并按绿色 **OK** 勾号。例如，尝试设置 2mm 厚度并按 **OK**。您现在应该会看到 **Base Flange** 的厚度已从 1mm 变为 2mm。

