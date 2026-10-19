

## Section 3:
将 3D 零件
转换为钣金并
创建钣金外壳

除了从头创建钣金模型外，我们还可以将现有的 3D 设计转换为钣金模型。本节展示了两种不同的做法，并提供了一个实践示例，将本书中迄今为止学到的许多技能结合起来。

本节包含以下章节：

- Chapter 11, Converting to Sheet Metal Using the Insert Bends Tool
- Chapter 12, Building Sheet Metal Parts Using the Convert to Sheet Metal Tool
- Chapter 13, Practical Example: The Sheet Metal Enclosure

## 11
使用插入折弯工具将零件转换为钣金

具有恒定壁厚的 3D 模型（例如 *Shelled* 或 *Thin Feature* 零件），可通过使用 **Insert Bends** 工具转换为钣金 *parts*。本章介绍如何使用此工具创建钣金零件。

到本章结束时，学习者将能够自信地使用 Insert Bends 工具。

在本章中，我们将涵盖以下主要主题：

- 使用 Insert Bends 工具处理 Shelled 零件
- 使用 Insert Bends 工具处理 Thin Feature 零件

### 使用 Insert Bends 工具处理 Shelled 零件

到目前为止，在本书中，我们已经涵盖了大多数 SolidWorks 钣金工具，并且在几乎所有情况下，我们都使用 Base Flange 功能开始我们的模型。然而，也可以将现有的实体模型转换为钣金模型。这可以通过两种主要方式完成：使用 Insert Bends 工具或使用 Convert to Sheet Metal 工具。虽然这两种工具相似，但它们确实存在一些重要差异，这些差异将在本章和第 12 章 Building Sheet Metal Parts Using the Convert to Sheet Metal Tool 中分别进行解释。

我应该使用 Base Flange 还是将实体模型转换为钣金？
通常，使用 Base Flange 开始钣金模型是更好的选择，因为如果以后需要更改，它会为您提供灵活性。有时，创建实体模型然后将其转换为钣金似乎会更快，但从长远来看，一旦考虑到任何未来的修改，通常最好从 Base Flange 开始。

Insert Bends 和 Convert to Sheet Metal 工具在处理来自不同来源收到的预先存在的模型或您自己之前制作的模型时通常更有用。

Insert Bends 工具允许我们将实体模型（例如 Shelled 或 Thin Feature 零件）转换为钣金零件。它最适用于具有均匀（即恒定）壁厚的零件。如果您的模型完全是实心的或具有可变壁厚，那么通常最好使用 Convert to Sheet Metal 工具，该工具在第 12 章 Building Sheet Metal Parts Using the Convert to Sheet Metal Tool 中介绍。

为了演示 Insert Bends 工具的使用，首先，我们将创建一个简单的 Shelled 模型：

- 1. 启动一个新的零件文档并创建一个每边长 100 mm 的实体立方体。这可以通过在 Top Plane 上绘制 Center Rectangle，然后使用 Extruded Boss/Base 工具和 Mid Plane End Condition 来实现。

2. 通过选择 Shell 工具并将上表面选为要删除的面之一来掏空立方体并移除上表面（Figure 11.1）。将壁厚设置为 2mm。

![](img/4e9c736de6bac4449096a7b92c28af4c_223_0.png)

Figure 11.1: Shelling the cube to a wall thickness of 2 mm and removing the upper face

3. 现在，如果我们进入 Section View，我们可以看到立方体在四周都有恒定的 2 mm 壁厚（Figure 11.2）。这使其非常适合通过使用 Insert Bends 功能转换为钣金。

![](img/4e9c736de6bac4449096a7b92c28af4c_224_0.png)

Figure 11.2: The cube has a constant wall thickness

4. 关闭 Section View 并从 Sheet Metal 选项卡或工具栏中选择 Insert Bends 工具。

Insert Bends Property Manager 出现（Figure 11.3）。我们可以使用它来设置钣金零件的默认参数（例如 Bend Radius 和 Auto Relief）。请注意，Thickness 设置不可用，因为厚度由实体零件的实际厚度决定。

将 Bend Radius 设置为 2mm 并保持其他设置作为默认值。

![](img/4e9c736de6bac4449096a7b92c28af4c_225_0.png)

Figure 11.3: The Insert Bends Property Manager

5. 设置参数后，我们需要选择一个 Fixed Face 或 Edge。这将是不移动的面或边，可以将其视为类似于 Base Flange。

旋转立方体并选择底面作为 Fixed Face（Figure 11.4）。

![](img/4e9c736de6bac4449096a7b92c28af4c_226_0.png)

Figure 11.4: Selecting the underside face as the Fixed Face

6. 在单击 OK 勾选标记之前，我们必须首先在 Rip Parameters 部分中选择 Edges to Rip。

要将此 3D 设计转换为平板，我们需要展开立方体的垂直侧面。然而，它们目前是连接在一起的。因此，必须撕开某些边，以便展开立方体的侧面。如果我们把立方体想象成真实的物体（例如，如果它是由纸板制成的），那么在展开侧面之前，我们必须沿着四个垂直边撕开。因此，我们需要在 Rip Parameters 部分中选择这些边。

左键单击 Rip Parameters 下方的框以选择 Edges to Rip 框，然后选择立方体的四个垂直边。请注意，每个选定边上都会出现黄色箭头（Figure 11.5）。这些箭头指示撕裂的方向。

![](img/4e9c736de6bac4449096a7b92c28af4c_227_0.png)

Figure 11.5: Selecting the four vertical edges to rip

- 7. 我们还可以通过调整 Rip Parameters 部分底部的数值来设置 Rip Gap 距离。将其设置为 0.5mm。

- 8. 单击绿色 OK 勾选标记以创建 Insert Bends 功能。您可能会收到一条消息，告诉您已为一个或多个折弯制作了 Auto Relief 切割。

零件现在已转换为钣金零件，并且在 FeatureManager Design Tree 中出现了许多功能（Figure 11.6）。我们还可以看到，之前尖锐的下边缘现在已转换为折弯，并且在下角添加了 Auto Relief 切割。

![](img/4e9c736de6bac4449096a7b92c28af4c_228_0.png)

Figure 11.6: The Insert Bends features and model

查看 FeatureManager Design Tree（Figure 11.6 的左侧）向我们展示了刚刚发生的步骤指南。

首先，我们创建了立方体形状（Boss-Extrude1）并对其进行了 Shell 处理（Shell1）。接下来，Sheet-Metal 文件夹作为 Insert Bends 过程的一部分被添加。正如通常一样，此文件夹始终包含此零件的所有钣金参数。随后是 Rip 功能（Rip1），它在立方体的垂直边缘上切出撕裂。Flatten-Bends 操作（Flatten-Bends1）随后将立方体的侧面折下，而 Process-Bends 操作（Process-Bends1）则将折弯添加到模型中并将侧面折回。最后，添加了 Flat-Pattern 文件夹，这将允许我们在未来阶段如果需要时展平零件。如果我们展平模型（Figure 11.7），角落中的减力切口将更容易看到。

![](img/4e9c736de6bac4449096a7b92c28af4c_229_0.png)

Figure 11.7: The flattened model, showing the relief cuts in the corners

Rollback Bar（FeatureManager Design Tree 底部的蓝色水平条）可以通过这些功能向上拖动，让您逐步回溯模型并一次看到这些更改。

### 调整撕裂

使用 Insert Bends 工具时，我们还可以调整撕裂以指定其方向。这可以在最初创建功能时完成，也可以在以后通过编辑 Rip 功能来完成：

- 1. 确保您的模型处于未展平状态。
- 2. 在 FeatureManager Design Tree 中左键或右键单击 Rip 功能并选择 Edit Feature。
- 3. 撕裂方向可以通过选择一条边然后在左侧 Property Manager 中单击 Change Direction 按钮来更改，也可以通过单击 Graphics Area 中的黄色箭头来更改（Figure 11.8）。

