

4. 我们现在可以选取想要倒角的角。角可以直接左键点击，或者选择整个面，所有相关的角都会被倒角。这样，Break-Corner/Corner-Trim 工具比 Fillet 或 Chamfer 工具更有用，因为我们不必大幅缩放来选择法兰上非常短的边；我们可以直接选择整个面。

如 Figure 9.9 所示，选择两个 Edge Flanges 和 Base Flange：

![](img/4e9c736de6bac4449096a7b92c28af4c_186_0.png)

Figure 9.9: Adding the Break Corner feature

5. 按下绿色的 OK 勾选标记以添加 Break-Corner/Corner-Trim 特性。

到目前为止，我们只是创建了外部倒角，但该工具也可以用于内部角，如下一节所示。

## Using the Break-Corner/Corner-Trim tool with internal corners

按照以下步骤使用此工具处理内部角：

- 1. 通过在上表面绘制矩形，然后选择 Base Flange/Tab 工具，为 Base Flange 添加一个 Tab（Figure 9.10）：

![](img/4e9c736de6bac4449096a7b92c28af4c_187_0.png)

Figure 9.10: Adding a Tab to the Base Flange

- 2. 再次选择 Break-Corner/Corner-Trim 工具。

3. 使用与之前相同的设置（**Fillet**，5mm size），再次选择 **Base Flange** 面。注意只有 *external* 角被选中（Figure 9.11）：

![](img/4e9c736de6bac4449096a7b92c28af4c_188_0.png)

Figure 9.11: Selecting the Base Flange face

4. 放大并手动选择两个 *internal* 角（Figure 9.12）。创建内部倒角会增加材料，而创建外部倒角会移除材料。

![](img/4e9c736de6bac4449096a7b92c28af4c_188_1.png)

Figure 9.12: Manually selecting internal corners

5. 按下绿色的 **OK** 勾选标记以添加新的倒角。

除了在折弯零件上使用 **Break-Corner/Corner-Trim** 工具外，它也可以在展开模型上使用，以便快速一起选择所有角。

## Using the Break-Corner/Corner-Trim tool on flattened models

在许多情况下，你可能会在不添加任何倒角的情况下创建整个 Sheet Metal 模型，但在建模结束时希望全部添加它们，也许是为了便于制造。

在这些情况下，可以在展开零件后使用 **Break-Corner/Corner-Trim** 工具：

- 1. 从前一个示例继续，删除任何当前的 **Break-Corner/Corner-Trim** 特性，以便你仅保留 **Base Flange**、**Tab** 和两个 **Edge Flange** 特性。
- 2. 通过点击 **Flatten** 按钮展开模型。
- 3. 现在展开 **Corner** 工具的下拉菜单，你会看到只有一个选项可用，**Corner-Trim**。这与我们在上一节中使用的工具非常相似，但专门用于展开模型。选择 **Corner-Trim** 工具（Figure 9.13）：

![](img/4e9c736de6bac4449096a7b92c28af4c_189_0.png)

Figure 9.13: Using the Corner-Trim tool with a flattened part

- 4. 我们现在可以添加 **Relief Options** 并添加 **Break Corner Options**。我们将在下一节介绍 **Relief Options**，现在仅查看 **Break Corner Options**。

与之前一样，我们可以设置 **Break type** 和 **Size**。

设置为 **Filletted**，5mm size。

- 5. 接下来，选择要倒角的角。使用此工具在展开状态下，我们有两个新选项。点击 **Collect all corners** 按钮将自动选择模型中所有相关的角。我们还可以勾选 **Internal corners only** 框以仅选择内部角（外部角需要手动选择）。

点击 **Collect all corners** 以选择模型中的弯折。

> **Using the Collect all corners Button versus Selecting the Face**
> 在选择要倒角的角时，我建议使用 **Collect all corners** 按钮，而不是仅选择面。这是因为有时选择面也会错误地选择弯折内的角，而这些角不应该被倒角。

- 6. 按下绿色的 **OK** 勾选标记以倒角所选角。注意 **Corner-Trim** 特性现在出现在 FeatureManager Design Tree 中 **Flat-Pattern** 文件夹之后。

- 7. 通过再次选择 **Flatten** 按钮展开零件。注意倒角在模型中不再出现，并且 **Corner-Trim** 特性被抑制（关闭）。这是因为 **Corner-Trim** 特性是 **Flat-Pattern** 特性的 *child feature*。因此，如果 **Flat-Pattern** 特性被抑制（例如当模型展开时），则 **Corner-Trim** 特性也会被抑制。

以这种方式在展开后使用 **Corner-Trim** 工具是在建模结束时在 **Flat-Pattern** 文件夹之后向模型添加特性的少数情况之一。

如果需要在未展开状态下对角进行倒角，则可以使用本章前面描述的 **Corner/Corner-Trim** 工具，也可以使用 **Unfold** 特性展开零件，然后手动倒角所有角，然后使用 **Fold** 特性将零件折回。

在下一节中，我们将查看最后一个角工具，**Corner Relief**。

## Preparing your model for manufacturing using Corner Relief

**Corner Relief** 工具允许我们在模型中添加角的减力切口。这对于避免在两个或多个弯折相遇的狭窄角处的制造问题很有用。

该工具可在未展开或展开状态下使用，我们将首先查看未展开版本：

- 1. 从前一个示例继续，删除任何当前的 **Break-Corner/Corner-Trim** 特性，以便你仅保留 **Base Flange**、**Tab** 和两个 **Edge Flange** 特性。确保模型处于 Unflattened（即折叠）状态。
- 2. 从 **Corner** 工具下方的下拉菜单中选择 **Corner Relief** 工具。我们现在可以在 Property Manager 中设置一些选项（*Figure 9.14*）：

![](img/4e9c736de6bac4449096a7b92c28af4c_191_0.png)

- 3. **Scope** 部分仅指示我们正在处理的板件。这通常会自动预填，但如果在多板件零件中工作，则可以选择适当的板件。

- 4. 接下来，我们需要选择 **2 Bend Corner** 或 **3 Bend Corner**。在我们当前的模型中，我们只有一个 2 Bend Corner。这意味着角有两个彼此相邻的弯折（*Figure 9.15*）。在下一节中，我们将查看 3 Bend Corners。

![](img/4e9c736de6bac4449096a7b92c28af4c_192_0.png)

Figure 9.15: A 2 Bend Corner

选择 **2 Bend Corner** 然后在 **Corners** 部分点击 **Collect all corners** 按钮。

注意 **Define Corner** 框现在已填入来自弯折的两个面。

5. 我们现在可以设置 **Relief Options**。主要有五个选项，每个选项都会产生略有不同的切口样式。每个版本还具有多个子选项：

- **Rectangular**: 这将切出一个矩形区域。切口尺寸可以由 **Slot length** 框中的数值决定，也可以勾选 **Ratio to thickness** 框，根据板厚计算尺寸。

**Centered on bend lines** 选项使切口相对于弯折线居中，而 **Tangent to bend** 选项使切口与弯折线相切（Figure 9.16）。

**Add filleted corners** 按钮仅将减力切口的尖角圆化。

![](img/4e9c736de6bac4449096a7b92c28af4c_193_0.png)

Figure 9.16: The Centered on bend and Tangent to bend options

- **Circular**: 产生圆形切口。

- **Tear**: 创建撕裂减力；没有可用的子选项。

- **Obround**: 这会创建腰形（槽形）切口。

- **Constant Width**: 这会产生具有恒定宽度的线性和连续切口，且没有子选项。

尝试不同的 **Corner Relief** 切口类型，以在图形区域的预览中查看它们之间的差异。

6. 按下绿色的 **OK** 勾选标记以创建 **Corner Relief** 切口。

除了 2 Bend Corners 外，**Corner Relief** 工具也可以与更不寻常的 3 Bend Corners 一起使用。

