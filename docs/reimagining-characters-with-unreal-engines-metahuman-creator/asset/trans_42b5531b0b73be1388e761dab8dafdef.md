
这为艺术家们打开了机会之门，让他们能够在自定义面部雕塑或真实人物的数字扫描上使用所有很棒的 MetaHuman 相关工具和功能。因为我们可以使用真实人物的数字扫描，所以艺术家们能够创建出与演员甚至艺术家本人有着惊人相似度的 MetaHuman。

在本章中，我们将重点介绍如何创建和利用你自己的面部数字扫描，并将其应用到 MetaHuman 上。为此，我们将使用一款名为 KIRI Engine 的应用程序，它同时适用于 iPhone 和 Android。然后我们将导入数字扫描，以便与 Mesh to MetaHuman 插件配合使用。

因此，在本章中，我们将涵盖以下主题：

* 为 Unreal Engine 安装 Mesh to MetaHuman 插件
* 介绍并在智能手机上安装 KIRI Engine 应用
* 将扫描的网格导入 Unreal Engine
* 在 MetaHuman Creator 中修改面部网格

# 技术要求

就计算机性能而言，你需要满足 [*第 1 章*](#B17878_01.xhtml#_idTextAnchor015) 中详述的技术要求。

你还将需要：

* 一部至少有 200 MB 可用空间的 iPhone 或 Android 手机
* 稳定的网络连接，因为你将下载插件并向服务器上传数据
* 可访问用于安装 Unreal Engine 的 Epic Games Launcher
* KIRI 应用
* Mesh to MetaHuman 插件
* Unreal Engine 中的一个新项目

注意：你需要熟悉 [*第 2 章*](#B17878_02.xhtml#_idTextAnchor027) 中的 MetaHuman Creator 在线应用程序，但不需要打开前几章的任何项目。

# 为 Unreal Engine 安装 Mesh to MetaHuman 插件

要开始，请在 Unreal Engine 中创建一个新项目并保持打开状态。然后，要启用 Mesh to MetaHuman 插件，我们首先需要通过 Epic Games Launcher 从 Unreal Engine Marketplace 下载它。

当你找到 Mesh to MetaHuman 插件页面时，点击 **Install to Engine**（安装到引擎）按钮，如 *图 11.1* 所示：

![Figure 11.1: The MetaHuman plugin](image/B17878_11_01.jpg)

图 11.1：MetaHuman 插件

安装好 Mesh to MetaHuman 插件后，我们现在将进入下一节，开始使用 KIRI Engine 应用创建数字扫描的旅程。

# 在智能手机上介绍并安装 KIRI Engine

什么是 **KIRI** Engine？KIRI Engine 是一款属于 **摄影测量** 类别的应用程序。摄影测量是利用照片在 3D 应用程序中创建 3D 几何体的过程（在大多数情况下是自动完成的）。

在为 MetaHuman 创建面部扫描的背景下，摄影测量要求艺术家在 180 度的弧线范围内以及不同高度拍摄多张照片。从 *图 11.2* 中，你可以看到头部和肩膀的四个不同角度。

![Figure 11.2: Typical camera positions](image/B17878_11_02.jpg)

图 11.2：典型相机位置

我们只对面部正面感兴趣，这就是为什么我们只在 180 度的弧线范围内进行捕捉。获取高角度和低角度也很重要，以便我们获得下巴底部、鼻孔和额头的信息。

摄影测量拍摄技巧：

* 使用漫射（柔和）光照，使面部无阴影
* 让被摄对象保持静坐，下巴微微上抬
* 如果可能，使用偏振镜滤镜以最大程度减少皮肤上的反光

现在，让我们开始使用 KIRI。最近，KIRI Engine 在 macOS 或 Windows 上通过网页浏览器使用时已推出桌面版本，你可以在此下载：`https://www.kiriengine.app/web-version/`。然而，为了本书的目的，我将假设你拥有一部 Android 智能手机或 iPhone，并且你正在将 KIRI Engine 应用下载到智能手机上。

如果你有 Android 设备，可以免费从 Google Play 安装该应用，如下所示：

![Figure 11.3: KIRI Engine on Google Play](image/B17878_11_03.jpg)

图 11.3：Google Play 上的 KIRI Engine

既然你们中的许多人在 [*第 8 章*](#B17878_08.xhtml#_idTextAnchor120) *使用 iPhone 进行面部动作捕捉* 中已经用过 iPhone，你们会很高兴地知道不必跑去弄个 Android。通过前往 App Store，你可以在设备上下载 KIRI Engine 应用：

![Figure 11.4: KIRI Engine on the App Store](image/B17878_11_04.jpg)

图 11.4：App Store 上的 KIRI Engine

使用 KIRI Engine 非常简单，因为运行应用时可供选择的选项很少。当你首次打开 KIRI Engine 时，无论你使用的是 Android 还是 iPhone 设备，都会看到如 *图 11.5* 所示的简单界面：

![Figure 11.5: The KIRI Engine welcome screen](image/B17878_11_05.jpg)

图 11.5：KIRI Engine 欢迎屏幕

在这里，你将可以选择上传之前拍摄的照片，或使用 KIRI 应用拍照。选择后者，**Take photos**（拍照），屏幕将发生变化：

![Figure 11.6: The manual option](image/B17878_11_06.jpg)

图 11.6：手动选项

手动捕捉设置是默认设置。它由 *图 11.6* 左上角的静态相机图标表示。这是一个手动捕捉选项，意味着每次想拍照时，你都必须点击屏幕底部的大白色按钮。问题在于，每次点击智能手机屏幕都会引入轻微的相机抖动，在特定光照条件下会导致照片模糊。

点击右侧的电影摄像机图标即可访问自动捕捉设置，如 *图 11.7* 所示。使用此设置，你只需点击一次红色按钮即可开始捕捉。此设置每隔几秒拍摄一张照片（多达 70 张），如果你移动相机过快，还会发出警告。

![Figure 11.7: The auto-capturing option](image/B17878_11_07.jpg)

图 11.7：自动捕捉选项

注意录制按钮上方的进度线；这指示覆盖质量，即 **Acceptable**（可接受）、**Good**（良好）和 **Perfect**（完美）。**Acceptable** 结果将得到一个细节较少且容易出现块状感的 3D 模型，而 **Perfect** 照片结果将得到一个细节更丰富、更平滑的 3D 模型。

理想情况下，你应该让一张照片到下一张照片的重叠度尽可能高。前后相邻图像之间约 80% 的重叠度是一个好的经验法则。重叠是指每两三张连续图像应至少共享一个特征，例如眼睛或鼻子。

一旦你达到最大图像数量，并在面部周围达到了合理的覆盖程度，你将看到 *图 11.8* 所示的界面：

![Figure 11.8: The upload screen](image/B17878_11_08.jpg)

图 11.8：上传屏幕

在这个界面中，你将看到以下选项：

* **Face Counts**（面数）：用于创建 3D 模型的多边形数量。选择 **High**（高），以确保 MetaHuman 插件有足够的几何体可用（你不必担心生成过于稠密的几何体，因为 MetaHuman 设计用于处理多边形数量极高的高密度网格）。
* **Texture Quality**（纹理质量）：KIRI 不会生成超大的纹理贴图，如果你已在 Unreal Engine 中使用过 MetaHuman，则不会对 KIRI 创建的最高质量纹理贴图有任何问题。所以，再次选择 **High**（高）。
* **File Format**（文件格式）：虽然 Unreal Engine 可以导入 OBJ 和 FBX 两种文件格式，但据报告 FBX 格式的处理时间较长；Epic Games 建议使用 OBJ 以便插件快速工作，所以选择 **OBJ**。
