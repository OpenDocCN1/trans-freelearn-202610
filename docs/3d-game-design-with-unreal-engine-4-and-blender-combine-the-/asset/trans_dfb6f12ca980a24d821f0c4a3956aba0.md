
## 材质绘制进阶步骤

2.  现在找到 `Silver Armor` 材质并将其拖入 `Layer` 面板。确保它在图层堆栈中位于 `Steel dark stained` 之上。

3.  右键点击 `Silver Armor`，从菜单中选择 `Add White Mask`。

4.  回到工具栏，开启 `Polygon Fill` 工具。就像我们使用 `Quick Mask` 时一样，将 `Properties` 面板中的 `Color` 滑块完全向左滑动，然后点击 `UV` 按钮。现在选择 Artifact 顶部和底部零件的外侧壳体。同时也选择底部多边形。这将允许底层的钢铁图层透过 `Silver Armor` 图层显示出来。结果是齿轮和活塞现在是银色的，而外壳仍保持阴暗的金属色：
    ![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_38.jpg)

### 使用 Satin 通用材质为 Artifact 内部上色

5.  还剩最后一个部分！我们要给 Artifact 内部添加缎面材质。在图层堆栈中，Smart Materials 上方创建一个新图层并命名为 `Satin`。按 `T` 创建 `Quick Mask`，遮罩掉除 Artifact 内部以外的所有部分。在 `Shelf` 的 `Materials` 选项卡中，向下滚动并选择 `Satin Generic` 材质。接下来，将 alpha 设置为 `Shape`。最后，在视图的 UV 布局侧，点击并拖动画笔绘制 Artifact 内部面。这会将材质直接绘制到 UV 贴图上，其工作方式与在模型上绘画完全相同。

我们的材质制作完成了！是时候将这些很棒的纹理贴图从 Substance Painter 导出，为 Unreal 4 做好准备。

## 烘焙与导出纹理

1.  我们从烘焙 `Normal` 贴图开始。点击 `TextureSet Settings` 面板中的 `Bake Textures` 按钮。关闭除 `Normal` 以外的所有选项。保持所有选项为默认值，点击窗口底部附近的 `Bake Textures`。我们现在应该能在 `Additional Maps` 列表中看到新增的法线贴图：
    ![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_39.jpg)

### 打开 Export Textures 菜单

2.  接下来，点击 `File` 菜单并选择 `Export Textures`。点击 `Configurations` 选项卡。这里列出了几个不同的预设，根据我们的游戏资产接下来要去向哪里而定。虽然有 `Unreal Engine 4` 选项，但我们要点击窗口顶部附近的 `+` 来创建自己的预设。双击新预设并将其命名为 `Normal`。

3.  这个预设将仅用于导出法线贴图，不包含我们暂时不需要的其他贴图。在窗口的 `Output Maps` 部分，点击 `RGB` 按钮为法线贴图创建一个 RGB 通道。接下来，将窗口 `Converted Maps` 部分的 `Normal DirectX` 选项拖拽到我们刚创建的新通道中的 `RGB` 选项上。该通道将输出我们的法线贴图。最后，给该通道命名为 `Normal`。

4.  回到 `Export Textures` 菜单，切回 `Export` 选项卡。将 `Config` 下拉菜单改为我们新建的 `Normals` 选项，点击窗口底部的 `Export` 按钮。几秒后，我们应该会收到导出成功完成的消息。点击 `Open Folder` 选项以导航到我们的新法线贴图：
    ![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_40.jpg)

### 使用生成的 Normal map 测试 Artifact 是否保留了高度信息

5.  我们的法线贴图包含了存储在 `Height` 通道中的所有细节信息。在 `Shelf` 中，切换到 `Textures` 选项卡。现在打开包含新法线贴图的文件夹，将贴图拖入 `Textures` 选项卡。最后，点击 `TextureSet Settings` 面板 `Additional Maps` 部分的 `Normal Map`，从选项中选择我们的新法线贴图。我们可以通过前往 `Layers` 面板并关闭 `Height Info` 文件夹来测试。如果所有细节都保留下来了，我们就万事大吉了！

6.  接着，回到 `Bake Textures` 菜单，关闭除 `Ambient Occlusion` 以外的所有选项。这种贴图能增强游戏资产凹槽和缝隙中的阴影，Unreal 4 可以利用它。

7.  最后一次导出。前往 `File` 菜单再次选择 `Export Textures`。从菜单中选择 `Unreal Engine 4 (Packed)`。最后，点击 `Export` 按钮！

我们终于完成了这个东西的贴图制作！现在你已经对 Substance Painter 的工作流程有了很好的感觉，我鼓励你再为触手跑一遍这个流程。Substance Painter 有一些逼真皮肤的有趣选项，可能非常适合为我们资产的这部分带来诡异恐怖的感觉。

# 总结

哇，真是一段精彩的旅程！在本章中，我们探索了不同的 UV 展开技术，并将它们应用到神器的不同部分。然后我们将游戏资产的所有不同部件连接在一起，创建了一个统一的 UV 贴图。有了 UV 贴图，我们将神器导入 Substance Painter，这是一款利用**基于物理的渲染**（PBR）技术创建逼真材质解决方案的贴图绘制程序。在这个复杂的程序中，我们直接在神器部件上绘制新细节，并为它们涂上了一层令人惊叹的漆面，以增强神器的**邪恶外观**。在下一章中，我们将探索为神器制作动画的方法，并在**虚幻引擎 4** 中组装这个邪恶物体的最终版本。

神器已经走过了漫长的道路。最初只是绘图纸上的一个想法，现在已经变成了一个完整的 3D 模型，并带有纹理。然而，我们还没有完成让这个被遗忘的遗物完全复活的工作。如果你还记得这个游戏资产的原始设计，神器必须能在玩家触碰时打开，使他们无意中成为其毁灭目的的帮凶。为此，我们需要动画！Blender 拥有一套完整的动画工具，完全胜任这项工作。使用关键帧，我们将创建一个简单的打开动画，并准备将神器导出到我们的空间站关卡中。在本章中，我们将涵盖以下主题：

*   Blender 如何处理动画？
*   绑定与使用关键帧
*   使用 Blender 的动画工具套件

# Blender 如何处理动画？

你可能还记得在 [第 2 章](#ch02.xhtml "第 2 章. 启动我们的第一个项目")，*启动我们的第一个项目* 中，我们简要涉及了关键帧，那些我们用来为物体制作动画的时间记录点。Blender 使用的工具与在 UE4 中使用 `Timeline` 和 `Matinee` 非常相似，所以让我们花一点时间建立在这些知识之上。请记住，关键帧代表动画中时间上的重要位置。就我们的门而言，就是打开和关闭的位置。对于电梯，就是上升和下降的位置。

这些关键帧帮助虚幻引擎和 Blender 创建所谓的 `Tweens`（补间动画）——即主要关键点之间所有那些细微的移动。连同关键帧一起，30 个关键帧组成一秒的动画。在兔八哥、飞毛腿鸟和《摩登原始人》的全盛时期，动画工作室有高级动画师，他们创建所有关键帧。然后将这些交给初级动画师手工绘制所有补间帧。这个过程曾经非常漫长且劳动密集。现在我们有计算机为我们计算所有中间帧，这个过程可以非常轻松地完成。

那么关键帧可以记录什么呢？

![Blender 如何处理动画？](OEBPS/Images/B05189_08_01.jpg)

### 插入关键帧菜单
