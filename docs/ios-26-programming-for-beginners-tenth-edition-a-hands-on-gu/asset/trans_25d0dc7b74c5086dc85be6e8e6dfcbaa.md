

让我们研究一下这个屏幕的不同部分。

屏幕底部的标签栏显示了 Journal 和 Map 按钮。Map 按钮被选中，你可以看到一个 MKMapView 实例（地图视图）在屏幕上显示地图，并用图钉标示日记条目。

点击一个图钉会显示一个标注，点击标注中的按钮会显示该日记条目的 Journal Entry Detail 屏幕。

这就完成了对该应用的介绍。现在，是时候开始为它构建 UI 了！

## 修改你的 Xcode 项目

现在你已经知道应用的屏幕会是什么样子，可以开始构建它了。如果尚未完成，请打开你在第 1 章 Hello, Xcode 中创建的 JRNL 项目：

![](img/0972ba626734a3986e095c788777818e_175_0.png)

图 10.10：JRNL 项目

验证 **Destination** 菜单中是否已选择 **iPhone 16e**。构建并运行你的应用。你会看到一个空白的白色屏幕。如果在 Project navigator 中点击 **Main** storyboard 文件，你会看到它包含一个包含空白视图的单个场景。这就是运行应用时你只看到空白白色屏幕的原因。

要配置 UI，你将使用 Interface Builder 修改 **Main** storyboard 文件。Interface Builder 允许你添加和配置场景。每个场景代表用户会看到的一个屏幕。你可以向场景中添加视图和按钮等 UI 对象，并使用 Attributes inspector 按需配置它们。

> 要了解如何使用 Interface Builder 的更多信息，请访问此链接：https://help.apple.com/xcode/mac/current/#/dev31645f17f。

现在，你将把现有场景嵌入到标签栏中，并在其中添加另一个场景。标签栏场景将在屏幕底部显示一个带有两个按钮的标签栏。点击一个按钮会显示与其关联的屏幕。这些屏幕对应于应用介绍中显示的 Journal List 和 Map 屏幕。让我们在下一节中看看如何实现。

## 设置标签栏控制器场景

正如你在应用介绍中所看到的，JRNL 应用在屏幕底部有一个带有两个按钮的标签栏，用于显示 Journal List 和 Map 屏幕。你将把现有的视图控制器场景嵌入到标签栏中，并在标签栏中添加第二个视图控制器场景。请按照以下步骤操作：

+   1. 在 Project navigator 中点击 **Main** storyboard 文件：

![](img/0972ba626734a3986e095c788777818e_176_0.png)

图 10.11：Project navigator 中已选择 Main storyboard 文件

Main storyboard 文件的内容会出现在 Editor 区域中。

+   2. 点击 Document Outline 按钮以显示文档大纲，如果它不可见的话：

![](img/0972ba626734a3986e095c788777818e_177_0.png)

图 10.12：Editor 区域中显示 Document Outline 按钮

+   3. 在文档大纲中选择 View Controller：

![](img/0972ba626734a3986e095c788777818e_177_1.png)

图 10.13：文档大纲中已选择 View Controller

+   4. 你将把现有视图控制器场景嵌入到标签栏控制器场景中。从 Editor 菜单中选择 Embed In | Tab Bar Controller：

![](img/0972ba626734a3986e095c788777818e_177_2.png)

图 10.14：Editor 菜单中已选择 Embed In | Tab Bar Controller

你会在 Editor 区域中看到一个新的标签栏控制器场景出现。

5. 点击 Editor 区域左下角的 + 按钮以显示库：

![](img/0972ba626734a3986e095c788777818e_178_0.png)

图 10.15：Editor 区域中显示 + 按钮

库允许你挑选要添加到场景的 UI 对象。

6. 在库的过滤字段中输入 view con。View Controller 对象将出现在结果列表中：

![](img/0972ba626734a3986e095c788777818e_178_1.png)

图 10.16：库中已选择 View Controller 对象

7. 将 View Controller 对象拖到 storyboard 以添加新的视图控制器场景，并将其定位在现有视图控制器场景下方：

图 10.17：Main storyboard 文件中已添加视图控制器场景

8. 点击 - 按钮以缩小，并重新排列 storyboard 中的场景，以便标签栏控制器场景和视图控制器场景都可见：

图 10.18：Editor 区域中显示缩放按钮

如果 – 和 + 按钮不可见，请尝试放大 Xcode 窗口。你也可以使用 Navigator 和 Inspector 按钮隐藏 Navigator 和 Inspector 区域。

9. 在文档大纲中选择 Tab Bar Controller。按住 Ctrl 并从 Tab Bar Controller 拖动到新添加的视图控制器场景：

图 10.19：Editor 区域显示拖动目标

10. 会出现一个 segue 弹出菜单。从此菜单中选择 view controllers：

图 10.20：Segue 弹出菜单

2. 点击 Item Scene 下带有星形图标的 Item 按钮。然后，点击 Attributes inspector 按钮：

![](img/0972ba626734a3986e095c788777818e_183_0.png)

图 10.24：已选择 Attributes inspector

3. 在 Bar Item 下，将 Title 设置为 Journal，将 Image 设置为 person.fill：

![](img/0972ba626734a3986e095c788777818e_183_1.png)

图 10.25：Attributes inspector 中 Title 设置为 Journal 且 Image 设置为 person.fill

## 将视图控制器嵌入导航控制器

正如你在应用介绍中所看到的，Journal List 和 Map 屏幕在屏幕顶部都有一个导航栏。为了为两个屏幕添加导航栏，你将把 Journal 和 Map 场景的视图控制器嵌入到导航控制器中。这将在显示 Journal List 和 Map 屏幕时使导航栏出现在屏幕顶部。请按照以下步骤操作：

+   1. 在文档大纲中点击 Journal Scene：

![](img/0972ba626734a3986e095c788777818e_185_0.png)

图 10.28：文档大纲中已选择 Journal Scene

+   2. 从 Editor 菜单中选择 Embed In | Navigation Controller：

![](img/0972ba626734a3986e095c788777818e_185_1.png)

图 10.29：Editor 菜单中已选择 Embed In | Navigation Controller

图 10.31：Editor 区域显示已添加的导航控制器场景

Journal List 屏幕和 Map 屏幕现在都有导航栏，但由于它们与背景颜色相同，因此在屏幕上并不明显。你将为每个场景的导航项设置标题以区分它们。

+   5. 在文档大纲中选择第一个 View Controller Scene 的 Navigation Item。在 Attributes inspector 中，在 Navigation Item 下将 Title 设置为 Journal：

图 10.32：Attributes inspector 中 Title 设置为 Journal

6. 在文档大纲中选择第二个 View Controller Scene 的 Navigation Item。在 Attributes inspector 中，在 Navigation Item 下将 Title 设置为 Map：

![](img/0972ba626734a3986e095c788777818e_188_0.png)

图 10.33：Attributes inspector 中 Title 设置为 Map

7. 构建并运行你的应用并点击每个标签栏按钮以显示相应的屏幕。请注意，每个屏幕都在导航栏中显示标题。

将视图控制器嵌入导航控制器会将该视图控制器添加到导航控制器的 viewControllers 数组中。导航控制器随后在屏幕上显示视图控制器的视图。导航控制器还在屏幕顶部显示一个带有标题的导航栏。

恭喜！你刚刚为你的应用配置了标签栏和导航控制器！

你可能已经注意到，Interface Builder 中表示的屏幕与你在 Destination 菜单中选择的 iPhone 型号不匹配，而且你可能会发现 minimap 显示会妨碍在应用中排列屏幕。让我们对 Interface Builder 进行一些额外配置来修复这个问题。

## 配置 Interface Builder

即使你已将 Simulator 配置为对你的应用使用 iPhone 16e，Interface Builder 中显示的场景仍是针对不同 iPhone 型号的。你可能也希望隐藏 minimap 显示。让我们配置 Interface Builder 中的场景以使用 iPhone 16e SE 并隐藏 minimap 显示。请按照以下步骤操作：

+   1. Main storyboard 文件应仍处于选中状态。要配置 Interface Builder 中场景的外观，请点击设备配置按钮：

![](img/0972ba626734a3986e095c788777818e_189_0.png)

将出现一个显示不同设备屏幕的弹出窗口。

+   2. 从此弹出窗口中选择 iPhone 16e，然后点击 Editor 区域中的任意位置以关闭它：

![](img/0972ba626734a3986e095c788777818e_189_1.png)

storyboard 中场景的外观将更改为反映 iPhone 16e 的屏幕。

