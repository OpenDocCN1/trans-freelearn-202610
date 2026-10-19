

# 使用 3 个弯角的 Corner Relief 选项

3 个弯角在钣金模型中相对较少见，但我们现在简要看一下：

+   1. 在之前的模型基础上，添加两个进一步的边法兰，如图 9.17 所示。这将创建一个 3 个弯角（我们刚刚添加了两个更多的弯角，而第一组边法兰之前已经有一个弯角）。

![](img/4e9c736de6bac4449096a7b92c28af4c_194_0.png)

+   2. 我们现在可以使用相同的角工具，因此从 Corners 下拉菜单中选择 Corner Relief 工具。

+   3. 在 Corner Type 部分，选择 3 Bend Corner。

+   4. 按下 **Collect all corners** 按钮并注意 3 Bend Corner 被选中，并且所有三个面都被添加到 **Define Corner** 部分。
+   5. 我们现在可以使用 **Relief Options** 部分以与 2 Bend Corners 相同的方式添加角减料切口。

以这种方式，我们可以使用多个 **Corner Relief** 特性为不同角类型添加减料切口，当模型处于折叠状态时，但我们也可以使用 **Corner Relief** 工具处理展开的零件。

## 向展开模型添加 Corner Relief 切口

有时你可能希望先完成大部分钣金模型，然后在建模结束时再向整个设计添加减料切口。使用此方法时，将切口应用于展开模型可能更容易：

+   1. 在大部分建模完成后，通过按下 **Flatten** 按钮展开模型。
+   2. 点击 **Corner** 工具下方的下拉菜单。选择 **Corner-Trim** 选项——这是展开状态下唯一可用的选项。这是我们在上一节中用于消除尖角的相同工具，但也可以用于添加角减料。
+   3. 在 **Relief Options** 部分下点击 **Collect all corners** 按钮。注意在展开状态下，我们不需要指定 2 或 3 个弯角——它们都被同等对待。
+   4. 设置 **Relief Type** 和 **Size**。这可以是 **Circular**、**Square** 或 **Bend Waist**（图 9.18）：

![](img/4e9c736de6bac4449096a7b92c28af4c_195_0.png)

+   5. 按下绿色的 **OK** 勾选标记以添加减料切口。注意这些减料仅在展开状态下出现。如果模型被展开，则 **Corner Relief** 特性将被抑制。

以这种方式，减料切口可以一次性全部轻松添加。

## 总结

在本章中，我们查看了四个 **Corner** 工具。第一个 **Closed Corner** 允许我们减少相邻法兰之间的角间隙，而 **Welded Corner** 工具则可以完全消除这些间隙。**Break-Corner/Corner-Trim** 工具加快了尖角的去除速度，最后一个工具 **Corner Relief** 添加了可能因制造原因而需要的减料切口。

在下一章中，我们将探讨如何通过创建和使用 **Forming Tools** 为我们的平板添加 3D 细节。

# 10
使用
Forming Tools
向模型添加 3D 细节

到目前为止，我们已经学习了如何通过使用诸如边法兰等特征向平板添加三维（3D）细节。然而，也可以通过使用 Forming Tools 添加 3D 特征。这些是专门制作的工具，通常用于将形状压入平板。

本章解释了如何使用 SolidWorks 设计库中现有的 Forming Tools，以及如何修改 Forming Tools 并创建和使用完全定制的 Custom Forming Tools。

在本章中，我们将涵盖以下主要主题：

+   - 使用 Forming Tools
+   - 修改现有 Forming Tools
+   - 创建 Custom Forming Tools

## 使用 Forming Tools

Forming Tools 是形状工具，也称为模具，在高压下压入平板，留下压痕。一些 Forming Tools 甚至可以在板材内创建切口，以及形状压痕。这些形状的例子包括诸如凹坑、百叶窗（形状通风口）甚至徽标和文字等事物。你可以在图 10.1 中看到一些使用成型工具创建的形状示例：

![](img/4e9c736de6bac4449096a7b92c28af4c_198_0.png)

图 10.1：使用成型工具创建的形状示例

SolidWorks 设计库包含许多预先制作的 Forming Tools，可以在处理钣金模型时轻松使用。由于这些工具在形状和尺寸方面范围相当有限，它们可能对你的需求没有太大用处，但它们可以被修改；本章后面的部分将展示如何做到这一点，以及如何创建完全定制的 Custom Forming Tools。

为了演示 Forming Tools 的使用，我们首先将制作一个简单的钣金零件，如下所示：

+   1. 启动一个新的零件文档并使用 Center Rectangle 工具创建一个固定在原点且尺寸为 50x50mm、厚度为 1mm 的 Base Flange。可以使用默认的钣金属性。
+   2. 通过点击屏幕右侧任务窗格中的图标（看起来像一本书）打开设计库，如图 10.2 所示。
+   如果你看不到任务窗格，则可以通过右键单击命令管理器并确保在 Tools | Task Pane 旁边有复选标记来打开它。

![](img/4e9c736de6bac4449096a7b92c28af4c_199_0.png)

图 10.2：打开设计库选项卡

3. 在任务窗格中展开设计库文件夹并展开 Forming Tools 文件夹。你现在应该会看到许多不同的 Forming Tool 类型，例如 embosses 和 ribs。点击 embosses 并注意任务窗格底部将显示各种可用的压花工具。注意：根据你的设置和 SolidWorks 版本，你的设计库可能与图 10.3 中的外观略有不同。

![](img/4e9c736de6bac4449096a7b92c28af4c_200_0.png)

图 10.3：展开设计库和 Forming Tools 文件夹

### 如果设计库不可见

如果在任务窗格中看不到设计库，则需要重新链接 SolidWorks 到正确的文件夹。

这可以通过转到 Tools | Options | System Options | File Locations 来完成。

然后，在下拉列表中找到 Design Library 并确保你的设计库文件夹的位置出现在 Folders 框中。如果文件夹不在那里，点击 Add 并找到你计算机硬盘上设计库的位置。

默认情况下，此位置通常为 C:\ProgramData\SolidWorks\SolidWorks 20xx\Design Library。

（注意 Program Data 文件夹通常是隐藏文件夹，因此你可能需要调整 Windows 资源管理器视图设置以显示隐藏文件夹。）

然后，按 OK 添加设计库文件夹，它现在应该出现在任务窗格中。

要使用 Forming Tools，可以通过按住鼠标左键并将所需工具拖到板材面上将其拖入模型。

+   4. 作为示例，在任务窗格的 forming tools 文件夹中选择 embosses 子文件夹，然后左键单击并将 counter sink emboss 工具拖到 Base Flange 的顶部面上。

现在图形区域中应该会出现一个黄色预览，显示将在模型上制作的压痕，如图 10.4 所示：

![](img/4e9c736de6bac4449096a7b92c28af4c_201_0.png)

图 10.4：将 Forming Tool 拖到模型上

### 你是否尝试制作派生零件？

如果你尝试将工具拖到板材上并收到消息 Are you trying to make a Derived Part?，这只是意味着 SolidWorks 不认识你正在使用 Forming Tools。为此，请通过选择 Cancel 关闭错误消息框。然后再次打开任务窗格，右键单击 Forming tools 文件夹，并在 Forming Tools Folder 选项旁边放置一个复选标记。这将确保 SolidWorks 知道文件夹内的所有零件都应被视为 Forming Tool 零件。

5. 按下绿色的 OK 勾选标记以插入 counter sink emboss 特性。我们现在应该会看到板材已被选定工具的形状变形，并且在压花区域中央有一个孔，如图 10.5 所示。如果我们查看设计库中的各种 Forming Tools（图 10.4——右下角），我们可以看到它们是黄色或红色着色的。黄色面表示工具的面将变形（或弯曲）板材，而红色面表示将被切除的材料（有关更多信息，请参阅本章后面 Creating Custom Forming Tools 部分）：

![](img/4e9c736de6bac4449096a7b92c28af4c_202_0.png)

图 10.5：Forming Tool 特性

我们可以以通常的方式编辑 Forming Tool 特性，或者我们可以展开该特性并编辑特性下方的定位草图，如果我们只是想更改该特性的位置。

6. 在 FeatureManager 设计树中左键单击 counter sink emboss 特性并选择 Edit Feature。

Forming Tool 特性类似于 Hole Wizard，因为它包含一个 Type 选项卡，可用于调整实际工具细节，并且还包含一个 Position 选项卡，允许定位一个或多个实例。

Type 选项卡（图 10.6）提供以下选项：

+   - Placement Face：这设置了工具将被压入的面。
+   - Rotation Angle：工具可以在此处旋转到任意角度。它也可以被翻转，允许工具从板材的另一侧压入。

