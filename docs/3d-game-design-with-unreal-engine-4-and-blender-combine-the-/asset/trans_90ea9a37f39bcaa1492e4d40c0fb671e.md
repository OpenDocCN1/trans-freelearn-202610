

## UV 展开

3. 在 3D 视口（右侧），你会注意到 `Tools` 面板已变为 `Shading/UV` 工具。在 `UVs` 标题下有一个 `UV Mapping` 选项下拉菜单。这些选项是我们开始对物体进行 UV 展开的地方。全选所有多边形后，打开菜单并选择 `Smart UV Project` 选项：

   ![UV unwrapping our game asset](OEBPS/Images/BO5189_03_20.jpg)

   使用 Smart UV Project 的基础 UV 布局

4. `Smart UV Project` 是 Blender 的自动展开工具。大多数 3D 创作套件都有类似工具，其输出结果会因物体复杂程度而异。对于像我们这样简单的箱子，效果相当不错。对于更有机的物体，或复杂的机械、建筑，可能很难获得理想结果。在 `Smart UV Project` 菜单中，我们将使用默认设置。点击 `Ok`，它就会展开你的物体。

5. `Smart UV Project` 将我们的物体压平并分离成了 UV 岛。UV 岛是物体的一部分。就我们的目的而言，Blender 分离物体的方式效果很好。如果我们制作更复杂的物体，可以将每个部分分离、分组使其更合理，甚至将一些部分缝合回去。稍后在本书创建更复杂物体时，这一步将变得必要。

6. 为了提高贴图质量，我们可以使用 `UV Layout` 视口底部 `UVs` 菜单中的 `Pack Islands` 选项。选择此选项将调整所有 UV 岛的大小，使其在不拉伸的前提下占据尽可能多的空间。

7. 既然我们在游戏引擎中工作，就需要第二个 UV 通道（或坐标集）来存储光照信息。要添加另一个 UV 通道，我们需要返回 `Scene Layout` 下拉菜单并选择 `Default`：

   ![UV unwrapping our game asset](OEBPS/Images/BO5189_03_21.jpg)

   创建第二个 UV 贴图通道

   在 `Properties` 面板中，点击 `Object Data` 图标。在 `UV Maps` 标题下，点击加号按钮以创建另一个 UV 通道。

8. 在这个第二个 UV 通道上，我们将添加光照贴图 UV。它们与用于纹理的 UV 布局不同，我们不关心所有部分是否缩放到相同数值。我们只关心它们占据尽可能多的空间以存储光影信息。Blender 的 `Lightmap Pack` 工具可以轻松完成此工作。返回 `Scene Layout` 下拉菜单中的 `UV Layout` 选项。`UV Layout` 视口右下角显示当前活动的 UV 通道。它会显示 `UVMap`。点击它并选择 `UVMap.001`。这是我们的新通道：

   ![UV unwrapping our game asset](OEBPS/Images/BO5189_03_22.jpg)

   使用 Lightmap Pack 工具创建虚幻 4 可用的光照贴图

9. 转到 3D 视口侧边的 `Tools` 面板，从 `UV Mapping` 菜单中选择 `Lightmap Pack`。默认选项也可以。点击 `Ok`。现在展开完成。返回 `Default` 屏幕布局：

   ![UV unwrapping our game asset](OEBPS/Images/BO5189_03_23.jpg)

   将 UV 纹理网格作为贴图添加到箱子上

10. 为了测试 UV 布局，我们可以添加之前下载的彩色 UV 纹理网格。这能轻松显示纹理是否有拉伸。如果我们手动展开，这张贴图将帮助我们拼凑 UV 贴图，确保所有元素朝向一致并最小化拉伸。点击 `Properties` 面板工具栏上的 `Textures` 按钮。在菜单的第一部分，进入 `Type` 下拉菜单并选择 `Image or Movie`。这将打开菜单的 `Image` 部分，允许你点击 `Open` 按钮。导航到存储图像的位置并选择它。

11. 点击 `Properties` 面板工具栏中 `Texture` 图标旁边的图标。这是 `Materials` 面板。剩下的就是将材质 `Assign` 给物体。点击 `Select` 选项选择所有多边形，然后点击 `Assign`。这会将材质添加到物体上。它会根据我们的 UV 贴图拉伸覆盖在物体上。

12. 显然，我们不想在虚幻环境中放一个五颜六色的盒子。在下一节中，我们将讨论如何给盒子添加基础材质，以及创建细节纹理的其他技术。

## 基础纹理技术

正如我之前提到的，纹理能让游戏物体在关卡中脱颖而出。它们可用于添加各种我们没时间建模的元素，以及颜色、磨损和光照信息。给游戏物体制作纹理有很多方法。想利用虚幻材质的基础物体可直接在关卡中应用一个。Blender 通过其纹理绘制模式提供了程序内的绘图工具。最后，还有使用其他程序创建纹理的选项。我们的 UV 贴图可以导出到 `Adobe Photoshop` 或 `GIMP` 等程序，在那里我们可以使用颜色工具绘制纹理，或使用照片创建逼真效果。我也想谈谈几年前出现的一种新型工具。`Substance Painter` 是一款允许你直接在 3D 物体上绘制纹理的应用程序，很像 Blender 的纹理绘制模式，但它拥有专为数字娱乐行业打造的选项和工具：

![Basic texturing techniques](OEBPS/Images/BO5189_03_24.jpg)

展示虚幻 4 中的新箱子

虚幻通过 `Starter Content` 文件夹提供了多种可用材质，既然我们的 UV 贴图会随物体一起导入，只需将材质拖放到箱子上即可。这些材质本身在箱子上看起来很棒，肯定能提供我们想要的关卡深度和特色。然而，既然材质不是专门为我们的箱子制作的，它们无法真正提供我们之前讨论的那些额外细节。下一章将物体导入虚幻时，我们将首先尝试这条路径：

![Basic texturing techniques](OEBPS/Images/BO5189_03_25.jpg)

Blender 的纹理绘制模式

Blender 自带纹理绘制模式。此模式可用于在物体上绘制纯色和图像。生成的纹理随后可从程序导出并在虚幻中应用。使用此模式让我们无需其他程序即可为箱子创建纹理，但因为 Blender 不是专业的图形编辑程序，其提供的工具有限：

![Basic texturing techniques](OEBPS/Images/BO5189_03_26.jpg)

Adobe Photoshop 中显示的导出 UV 贴图

`Adobe Photoshop` 及其开源替代品 `GIMP` 是图形编辑程序，可用于多种数字绘图、照片编辑等。通过将 UV 贴图导入这些程序，我们可以使用其绘图工具创建详细纹理，随物体一起导入虚幻。多年来这一直是创建游戏纹理最常用的方法。`Adobe Photoshop` 可在 `http://www.adobe.com/products/photoshop.html` 申请 30 天试用订阅。`GIMP` 可在 `http://www.gimp.org` 下载：

![Basic texturing techniques](OEBPS/Images/BO5189_03_27.jpg)

Allegorithmic Substance Painter 中的箱子

