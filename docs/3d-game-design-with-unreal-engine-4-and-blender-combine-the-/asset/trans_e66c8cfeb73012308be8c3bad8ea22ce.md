
# 法线贴图与 PBR 贴图工作流

## 理解法线贴图与 PBR

法线贴图存储了游戏资产高模版本的详细信息，随后可用于在同一游戏资产的低模版本上创建细节。这些详细信息存储在三个颜色通道中——红、绿、蓝，每个颜色对应三维空间中的不同轴向。这些信息随后用于模拟深度，能让游戏资产看起来比实际更加细致。

如今，生成这类纹理贴图的方法有很多。漫反射贴图和高光贴图可以使用 Photoshop 和 GIMP 等程序绘制在游戏资产 UV 的副本上。法线贴图通常需要使用单独的程序创建，例如免费的 Xnormal。然而，还有一种贴图技术正迅速流行起来。该过程称为**基于物理的渲染**，旨在利用漫反射贴图、高光贴图、粗糙度值和金属度值来模拟光线的真实行为，这与传统方法形成对比。过去游戏美术需要猜测光线如何与他们的游戏角色和物体交互，而 PBR 允许他们在游戏资产上绘制材质，由计算机模拟光照。结果往往更加准确，且在大多数光照情况下表现良好。PBR 最流行的工具之一是 Allegorithmic 的 Substance Painter。

## 修复触手几何体（重拓扑）

在处理 Substance Painter 之前，我们的模型有一个问题需要修复。当我们在触手上使用 `Decimate` 工具时，可能会产生一些难看的几何体，这对我们的三角形面数没有任何好处：

![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_24.jpg)

**是时候简化触手的几何体了**

这里有很多我们根本不需要的线条！这个过程称为重拓扑，简称重拓，简单来说就是回到物体上简化几何体，同时保持原始形状。让我们直接开始吧：

1. 选择触手并按 `Shift` + `H` 隔离它，然后再进入 `Edit` 模式。

2. 在 `Properties` 面板的 `Modifier` 菜单中，通过点击各自菜单中的眼睛图标，关闭 `Array` 和 `Curve` 两个修改器：

   ![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_25.jpg)

   *选择许多内部线条并溶解它们*

3. 观察几何体，有几条线我们可以移除，同时保持基本形状。选择所有类似于上图所示选择的线条。按 `X` 或 `Delete` 键并选择 `Dissolve` Edges。这将完全移除边及其对应的顶点：

   ![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_26.jpg)

   *好了！清理完毕。*

4. 对触手的每个主要面重复此过程，直到获得清晰简单的几何体！不用担心 UV 贴图，它已随我们的更改自动更新。

5. 最后，回到 `Object` 模式并重新启用修改器；我们还需要它们。我们需要将两个修改器都 `Apply` 到模型上，但顺序很重要。先应用 `Array` 修改器，再应用 `Curve` 修改器。最后，我们可以删除用作形状引导的 `Tentacle Curve`。它不再需要了。

## 为 Substance Painter 准备 UV 贴图

现在清理完毕，让我们看看如何使用 Substance Painter 为 Artifact 创建有趣的材质：

### 提示

Substance Painter 提供 30 天免费试用，可在 `https://www.allegorithmic.com/products/substance-painter` 下载。

![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_27.jpg)

*Substance Painter 中的 Artifact*

Substance Painter 将允许我们直接在 Artifact 上绘制颜色和细节，并根据结果创建所需的所有必要纹理贴图。该过程需要几个步骤：

1. 首先，我们将在 Blender 中调整 UV 贴图以准备导出。
2. 其次，我们将从 Blender 导出 FBX 文件并用它创建 Substance Painter 项目。
3. 第三，我们将在 Artifact 上绘制细节。
4. 第四，我们将添加颜色。
5. 最后，我们将从 Substance Painter 导出纹理贴图以在 Unreal 4 中使用。

这看起来是个漫长的过程，但结果绝对值得花时间。

为了让 Artifact 能在 Substance Painter 中工作，我们需要将所有部件合并并调整 UV 贴图：

1. 将所有对象合并是一个相当直接的过程。在 `Scene Outliner` 中，点击列表顶部的第一个部件，然后按住 `Shift` 键选择列表中的其他部件，确保跳过触手。我们将单独为该部件制作纹理。

2. 选中所有对象后，按 `Ctrl` + `J`。这将把它们合并为一个对象，并将所有 UV 贴图合并为一个。

3. 转到 `Screen Layout` 下拉菜单并选择 `UV Layout` 选项。选择我们新合并的 Artifact 并选择其所有面。我们的贴图将变得杂乱，所有展开的 UV 都开始重叠：

   ![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_28.jpg)

   *将 UV 岛排列成一个大的 UV 贴图*

4. 现在转到 `UV` 菜单并选择 `Average Island Scale`。这应该会调整所有 UV 岛的相对缩放。接下来，从 `UV` 菜单选择 `Pack Islands` 将它们全部打包到一对一的 UV 空间中。我们的结果应该类似于前面的截图：

   ![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_29.jpg)

   *将 Smoothing 选项设置为 Face*

好了，一切准备就绪可以导出。选中 Artifact，转到 `File` 菜单导出 FBX。确保在 `Export FBX` 选项中选择 `Selected Objects`。另外，转到该菜单的 `Geometries` 部分，将 `Smoothing` 下拉菜单改为 `Face`。导出！

### 提示

下一节需要 Substance Painter 的 30 天试用版以及他们在 YouTube 上的 Substance Painter 初学者贴图课程的演示文件。演示文件可在 `https://s3.amazonaws.com/alg-releases/public_releases/tutorials/scifi_crate/scifiCrate.zip` 下载。我强烈推荐这门课程，因为它能让你很好地入门 Substance Painter 和基于物理的渲染。

## Substance Painter 工作流

是时候打开 Substance Painter 了！对于有 Photoshop 经验的人来说，界面看起来和感觉都很相似。花几分钟探索一下信息面板。如果你觉得把它们放在别处会更好，可以像在 Unreal 中一样重新排列面板。只需抓住想移动的面板拖到新位置即可。

随着我们深入，我们不会涵盖 Substance Painter 的每个功能。我只会重点介绍完成 Artifact 所需的功能。如果你想了解更多，我强烈建议利用他们在 YouTube 上的免费教程。

我们需要做的第一件事是创建一个项目并导入 FBX 文件：

![Using different maps to create a more realistic look](OEBPS/Images/B05189_07_30.jpg)

*将 FBX 文件导入 Substance Painter*

1. 转到屏幕左上角选择 `File` 菜单。然后选择 `New`。将项目设置与前面截图中的设置匹配。点击窗口中的 `Select` 按钮将允许我们为项目选择 FBX 文件。
