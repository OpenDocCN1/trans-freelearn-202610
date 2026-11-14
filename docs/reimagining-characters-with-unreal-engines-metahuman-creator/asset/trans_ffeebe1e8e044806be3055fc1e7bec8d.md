
图 11.23：Mesh to MetaHuman 按钮

1. 选中后，你将获得一个 `Mesh to MetaHuman` 弹出对话框，如图 11.24 所示。只需点击 `OK`。

![图 11.24：你的 MetaHuman 现已可用](image/B17878_11_24.jpg)

图 11.24：你的 MetaHuman 现已可用

此时，我们无法看到 Mesh to MetaHuman 插件的进度，因为处理过程在云端进行。更具体地说，我们需要前往 MetaHuman Creator 在线工具查看结果。

在下一节中，我们将转到 Quixel Bridge，可以从你的 `Content` 文件夹或独立的 Quixel Bridge 应用程序访问。我们使用 Quixel Bridge 来访问我们的新 MetaHuman，以便使用在线 MetaHuman Creator 进行调整。

# 在 MetaHuman Creator 在线工具中修改面部网格

打开 Quixel Bridge 后，导航到 `My MetaHumans`。在那里你会看到一个新的黑白图标。如图 11.25 所示，该图标表明此 MetaHuman 是使用 `Mesh to MetaHuman` 插件创建的。点击黑白图标并导航到 `Start MHC` 按钮。

![图 11.25：MetaHuman Identity 图标](image/B17878_11_25.jpg)

图 11.25：MetaHuman Identity 图标

你将被带到熟悉的 MetaHuman Creator 在线应用程序，该应用在 [第 2 章](#B17878_02.xhtml#_idTextAnchor027) 中介绍过。正如图 11.26 所示，MetaHuman 没有皮肤且有些变形。最显著的是，上半部分头部不自然地突出。原因是使用 KIRI Engine 应用创建的面部网格扫描也扫描了我的头发；MetaHuman Identity 求解器无法确定头发实际是什么，导致生成了皮肤组织而不是头发。

![图 11.26：带有面部扫描的 MetaHuman Creator](image/B17878_11_26.jpg)

图 11.26：带有面部扫描的 MetaHuman Creator

头发问题有一个简单的修复方法。点击 `Face` 下的 `Custom Mesh` 选项，如图 11.27 所示。

![图 11.27：Custom Mesh 工具](image/B17878_11_27.jpg)

图 11.27：Custom Mesh 工具

如果你看左侧的面部缩略图，它被分割成各个组件，对应右侧我面部上色码的组件。

也要注意 `Region Influence` 滑块，我已将其降至 0。这意味着该部分网格的形状由 MetaHuman 模板决定，而不是由 KIRI Engine 扫描创建的面部网格决定。对比图 11.26 和图 11.27，你可以看到区别——前者的头部比后者块状得多。

如果你想进一步编辑，可以点击 `ENABLE EDITING` 按钮微调网格。不过，我建议你先改进扫描技术以获得更好的结果，将 `ENABLE EDITING` 选项作为最后的手段。

> **注意**
>
> 高质量照片能获得更好的结果。在本例中，光照条件不理想的智能手机导致了失真问题。在空白背景和非常柔和灯光的小型摄影棚中使用单反相机会获得更优的结果。此外，使用偏振滤镜进一步消除任何反光将是一个额外的加分项。

现在，让我们继续处理皮肤。我们从 KIRI Engine 扫描和随后导入 Unreal Engine 获得的纹理贴图和着色器已经丢失。这是一件好事。MetaHuman 拥有极其强大的皮肤着色器，能在 Unreal Engine 中非常真实地响应光照，我们将利用这一点。

因此，如图 11.28 所示点击 `Skin`，然后点击 `ASSIGN` 按钮分配肤色。

![图 11.28：分配肤色](image/B17878_11_28.jpg)

图 11.28：分配肤色

这将立即赋予我们带有 MetaHuman 所有着色器属性的肤色。我们现在对自定义面部扫描拥有的皮肤控制级别，与我们在 [第 2 章](#B17878_02.xhtml#_idTextAnchor027) 中首次创建 MetaHuman 时拥有的相同：

![图 11.29：编辑皮肤](image/B17878_11_29.jpg)

图 11.29：编辑皮肤

从图 11.29 也可以看出，默认光照给我们带来了相当柔和的光照结果。由于使用 KIRI Engine 在柔和光照下拍照是首选，在 MetaHuman Creator 中使用默认光照有助于匹配我们原始照片的肤色。

为了更好地判断模型是否工作正常，添加一些类似你自己的头发，就像我在图 11.30 中所做的那样：

![图 11.30：添加头发](image/B17878_11_30.jpg)

图 11.30：添加头发

因为 MetaHuman Creator 会持续自动保存你的 MetaHuman，你可以随时返回进行编辑。你也可以随时使用 Quixel Bridge 将带有自定义面部网格的新 MetaHuman 添加到项目中。

你可以花费大量时间修改自定义 MetaHuman，你应该花费的时间实际上取决于你的最终目标。如果你追求完美的相似度，你拥有 MetaHuman Creator 中所有的移动和雕刻工具，以及配合 KIRI 使用的所有照片作为参考，以实现更高的真实感。

# 总结

在本章中，我们了解了一个极好的额外功能，可用于创建与真人相似的角色。我们介绍了下载和安装 Mesh to MetaHuman 插件的过程，然后继续使用测量摄影学应用 KIRI Engine。之后，我们导入了 KIRI Engine 面部网格并利用 MetaHuman Identity 插件，学习了如何让自定义面部网格在 MetaHuman Creator 中工作，以及如何完善它并修复问题。

实际上，本章是一个额外章节，深入探讨了在撰写本书过程中推出的新工具。这是一个非常强大的新工具，不仅有潜力为数字角色替身创建合理的相似度，而且经过额外努力，还能为数字替身提供非常逼真的结果。

除了使用人物的 3D 扫描，你可能还会扫描雕塑，或重新利用其他 3D 模型，如奇幻角色甚至卡通风格角色。可能性是无限的。

这就是本书的结尾！我们涵盖了大量内容，考虑到我们在整本书中拥抱的技术和创新，这简直是轻描淡写。

作为一名艺术家和电影制作人，我的初衷是为其他艺术家和电影制作人汇编一份可供遵循和享受的指南。你们中的许多人精通 CGI 和动画，而许多人则处于旅程开始时的新手阶段。无论哪种方式，你现在都拥有一份分步指南，教你如何利用直到最近仍为大型动画工作室独有的技术，创造你自己的愿景、动画和娱乐作品！

# 索引

由于此电子书版本没有固定分页，下面的页码仅作为参考超链接，基于本书的印刷版。

A

ACES [293](#B17878_10.xhtml#_idIndexMarker593), [294](#B17878_10.xhtml#_idIndexMarker603)

ACES 色彩空间

对比 sRGB [294](#B17878_10.xhtml#_idIndexMarker601)

额外身体动作捕捉数据

添加 [272](#B17878_10.xhtml#_idIndexMarker545)

加法效果 [204](#B17878_07.xhtml#_idIndexMarker386)

高级渲染功能

ACES [293](#B17878_10.xhtml#_idIndexMarker594)

相机，添加和动画 [281](#B17878_10.xhtml#_idIndexMarker564)–[284](#B17878_10.xhtml#_idIndexMarker570)

调色 [293](#B17878_10.xhtml#_idIndexMarker596)

探索 [281](#B17878_10.xhtml#_idIndexMarker563)

灯光，添加和动画 [284](#B17878_10.xhtml#_idIndexMarker572)

电影渲染队列，使用 [287](#B17878_10.xhtml#_idIndexMarker580), [288](#B17878_10.xhtml#_idIndexMarker582)
