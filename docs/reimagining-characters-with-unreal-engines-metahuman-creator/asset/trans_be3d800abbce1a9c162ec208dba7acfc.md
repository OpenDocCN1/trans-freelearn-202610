

# 什么是 IK 链？

简单来说，IK 链就像任何其他链条一样，例如自行车链条，链条中的每个链节都与下一个链节有关系。这适用于身体的肢体，其中每根骨骼构成链条中的一个链节。因此，每个肢体，例如从肩膀到手，构成一条 IK 链。

`IK Rig` 所做的就是将每个 Rig 简化为以下五条 IK 链：

*   `Spine` 到 `Head`
*   `Left shoulder/clavicle` 到 `Left hand`
*   `Right shoulder/clavicle` 到 `Right hand`
*   `Left thigh` 到 `Left foot`
*   `Right thigh` 到 `Right foot`

> **注意**
>
> 请确保按照我在此处书写的顺序创建每条链，并且该顺序同时适用于源骨架和目标骨架。

## IK 重定向选项卡

为此，让我们仔细看看 `IK Retargeting` 选项卡。在*图 4.15* 中，既然我知道只需要五条 IK 链，我就点击了五次 `+Add New Chain` 按钮：

![图 4.15：源的 IK Retargeting 选项卡](image/B17878_04.15.jpg)

默认情况下，每条链的起始骨骼和结束骨骼都设置为 `None`。要更改此设置，请点击 `None`，然后输入 `Spine`。这将激活对所有可用骨骼的搜索；从结果中我们可以看到，此骨架有三个名为 `spine` 的可用骨骼。选择 `spine_01`。

![图 4.16：选择脊椎](image/B17878_04.16.jpg)

选择 `spine_01` 将使其成为给定的 `Start Bone`。对 `End Bone` 执行相同操作，但这次输入 `Head` 以完成链条。

并排显示两个 `IK Rigs` 后，我们就得到了第一条 IK 链的可视化指南，因为当我们选择一条 IK 链时，它会变成亮绿色。要选择一条链，请点击任意字段旁边的空白处（可以是 `Start Bone` 和 `End Bone` 字段之间，甚至只是垃圾桶图标旁边）。从*图 4.17* 中可以看到，我通过点击每个 Rig 的 `End Bone` 字段旁边，高亮显示了第一条 IK 链。这反过来会在两个视口中高亮显示该链条，这作为一个极好的视觉辅助工具。

![图 4.17：可视化第一条 IK 链](image/B17878_04.17.jpg)

现在，即使骨架不同，我们也为两个骨架都有了 IK 解决方案。

参考本节开头的列表，让我们继续第二条 IK 链。对于源 `IK Rig`，选择 `Clavicle Left` 作为 `Start Bone`，选择 `Hand Left` 作为 `End Bone`。然后，对目标 `IK Rig` 执行完全相同的操作。

按照表格和之前的说明，您将能够自行创建第三、第四和第五条 IK 链，以便您的源和目标 `IK Rigs` 看起来类似于*图 4.18*。正如您所见，我选择了最后一条 IK 链，即 `Left Leg`。您可以在两个 Rig 中看到 `Start Bone` 是 `thigh_l`，`End Bone` 是 `foot_l`，它们被选中，并且这在两个视口中都有体现，每个角色中相关的链条都高亮显示为绿色：

![图 4.18：源和目标 Rig 的 IK 链并排可视化](image/B17878_04.18.jpg)

源和目标 Rig 之间的这种精确匹配对于动画能够正确转换至关重要。完成此步骤后，在下一节中，我们将创建一个 `IK Retargeter`。

# 创建 IK Retargeter

完成了艰苦的工作，现在是时候创建一个工具，让这两个 `IK Rigs` 能够相互“对话”了。这实际上就是 `IK Retargeter` 的作用，它将动画从一个角色的 IK 链转换到另一个角色上。在本例中，我回到了我的 `Glenda` 文件夹来创建 Retargeter。那么，让我们执行以下步骤：

1.  就像创建 `IK Rig` 时一样，在角色的 `Blueprint` 文件夹中的任意位置右键单击，选择 `Animation`，然后选择 `IK Retargeter`，就像*图 4.19* 所示：

    ![图 4.19：创建 IK Retargeter](image/B17878_04.19.jpg)

2.  创建 `IK Retargeter` 时，系统会提示您选择要从中复制动画的 `IK Rig`。为了方便我们自己，我们之前已经将 `IK Rig` 标记为 `SOURCE_Mannequin`，以便轻松识别它作为源。您可以在*图 4.20* 的列表中看到它：

    ![图 4.20：选择源 IK Rig](image/B17878_04.20.jpg)

3.  选定后，我们将看到另一个显示源骨架的对话框。找到 `Target IKRig Asset` 选项，我在*图 4.21* 中用红色高亮显示了它（稍后我们会讨论用绿色高亮显示的选项）：

    ![图 4.21：IK Retargeter 对话框](image/B17878_04.21.jpg)

    其旁边的下拉列表将显示任何可用的 `IK Rigs`。我想要的目标 `IK Rig` 适当地标记为 `TARGET_Glenda`：

    ![图 4.22：在 IK Retargeter 中选择目标 IK Rig](image/B17878_04.22.jpg)

    选择 `Target IKRig Asset` 时，您应该会看到一个 `MetaHuman` 网格体出现在 `Mannequin` 旁边。如果没看到，请返回*图 4.21*，我在那里用绿色高亮显示了 `Target Preview Mesh` 选择框。

4.  现在，在*图 4.23* 中，您可以看到 `IK Retargeter` 为您提供了一个视口，其中 `Mannequin` 角色和 `MetaHuman` 角色并排在一起。此外，请注意屏幕底部有两个选项卡：`Chain Mapping` 和 `AssetBrowser`。确保您正在查看 `Chain Mapping` 选项卡，然后点击 `Auto-Map Chains` 按钮。

    ![图 4.23：IK Retargeter 中的链映射和自动映射选项](image/B17878_04.23.jpg)

5.  接下来，切换到 `AssetBrowser` 选项卡，您将看到可用的动画。在我的例子中，我只有一个名为 `Jog_Fwd` 的可用动画（您可能有更多）。点击其中一个，视口将更新并将动画应用到您的 `MetaHuman` 上。

    ![图 4.24：导出选定的动画](image/B17878_04.24.jpg)

6.  现在您可以导出该动画，以便将其重定向到您的 `MetaHuman`。如果您有多个可用动画，也可以一并选择。要导出，请选择动画并点击 `Export Selected Animations`。此时将出现一个对话框，要求您选择导出路径。我建议您选择您的 `MetaHumans` 文件夹来导出重定向后的动画，就像我在*图 4.25* 中所做的那样：

    ![图 4.25：选择保存重定向动画的目录](image/B17878_04.25.jpg)

> **注意**
>
> 选择文件夹并按下 `OK` 后，`IK Retargeter` 将使用动画名称创建一个新文件。在我的例子中，是 `Jog_Fwd`。创建的文件将带有 `Retargeted` 后缀，这是区分原始动画文件和重定向动画文件的便捷方式。

在此阶段，您已经在 `IK Retargeter` 视口中看到了 `MetaHuman` 上的动画。要将动画放入场景中，一种方法是打开您的角色 Blueprint（在我的例子中是 `BP_Glenda`）。

7.  打开 Blueprint 后，您现在可以在 `Animation` 部分下的 `Anim to Play` 下拉菜单中选择重定向后的动画，或者像*图 4.26* 所示那样，直接将重定向后的动画拖放到其旁边的框中：

    ![图 4.26：将重定向动画应用到角色 Blueprint](image/B17878_04.26.jpg)

