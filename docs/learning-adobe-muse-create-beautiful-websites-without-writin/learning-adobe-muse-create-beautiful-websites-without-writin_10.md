## 10

## Muse，认识 Adobe Creative Suite

在上一章中，我们使用 Muse 内置的小部件为页面添加了幻灯片。如果你使用 Adobe 的其他产品，如 Photoshop，你会很高兴知道 Muse 与 Adobe Creative Suite 中的其他产品紧密集成。Adobe Muse 不是 Adobe Creative Suite® 6 发行版的一部分，但它是 Adobe Creative Cloud 会员的一部分。然而，即使你使用的是 Photoshop 和 Fireworks 的旧版本，Muse 也能很好地处理你使用这些程序创建的文件。

在本章中，我们将了解如何使用 Photoshop 或 Fireworks 创建图形并轻松将其带入你用 Muse 创建的网站。我们将学习如何：

- 创建并添加一个 Photoshop 按钮
- 添加基于 Photoshop 或 Fireworks 的图形
- 使用 Edit Original 用 Photoshop 或 Fireworks 编辑图像

### 添加 Photoshop 悬停按钮

悬停按钮是由网页设计师创建的图形，它在页面访客和页面本身之间提供交互性。“悬停”一词源于这样一个想法：当访客将鼠标移过按钮时，视觉上会发生某些变化。

简单的悬停效果使用两张图像。第一张图像是我们首次加载页面时看到的图像。当我们将鼠标悬停在该按钮上时，第二张图像会迅速替换显示。当我们将鼠标移离按钮时，第一张图像会恢复显示。这种类型的交互性使用 JavaScript 创建，但与往常一样，Muse 在幕后完成工作并为我们编写代码。

> 悬停有时也被称为 mouseovers。

悬停不一定使用图像；最简单的悬停完全使用文本。文本可能只是改变颜色或以粗体、斜体或下划线显示。我们在第 6 章《Typography, Muse, and the Web》中添加菜单系统到页面时创建了一些基于文本的悬停效果。悬停的想法，无论是否基于图像，都是交互性为用户提供反馈并告诉他们这是一个按钮，将鼠标移过时会发生某些事情。而且通常点击它时也会发生某些事情。

### States

我们可以通过添加第三张图像来进一步扩展悬停按钮。第三张图像在用户实际点击按钮时出现。还有一个选项可以使用第四张图像，如果你希望按钮在被点击后以某种方式显示。

图像在网站上的显示方式可能会根据鼠标是否悬停在其上、点击它或移开而改变。每一种鼠标位置都被称为一个 State。我们可以在 Photoshop 或 Fireworks（或任何其他图形编辑器）中创建使用以下四种状态的按钮：

- Up：用户未与其交互时按钮的外观
- Over：用户将鼠标移过它时按钮的外观
- Down：用户选择（点击）它时按钮的外观
- Active：按钮被点击后其外观

> 你不必使用四种状态，甚至如果不想的话也不必使用三种，只要知道这些状态是可用的即可。

在下面的屏幕截图中，取自 Apple 网站，我们可以看到三种状态。当页面首次加载时，菜单上的所有按钮看起来都一样。它们都处于 Up State。当用户将鼠标悬停在菜单项上时，按钮会变为深灰色。当用户点击按钮时，按钮图像顶部和右侧的一些阴影给人一种按钮被按下下来的印象。

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_222_0.png)

### 在 Photoshop 中创建具有多种状态的悬停按钮

在本节中，我们将使用 Photoshop 创建一个类似的具有三种状态的按钮。如果你没有 Photoshop 的副本，可以跳到下一节，我们将在其中将按钮添加到页面，你可以使用提供的 .PSD 文件。

1. 打开 Photoshop 并创建一个新的图像，宽度和高度分别为 200 像素 x 75 像素。
2. 在图层面板中，点击 New Group 图标。点击图层面板中的新组名称并将其重命名为 Up。
3. 在工具面板上，将前景色设置为红色。

4. 选择 **Rounded Rectangle Tool**（它可能隐藏在矩形形状工具下）。在 Photoshop 界面顶部的工具选项栏上，确保第一个图标 **Shape Layers** 已被选中。拖放一个圆角矩形到文档窗口。

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_223_0.png)

一个新的形状图层已添加到 **Up** 组。现在我们将添加一些文本。

5. 在工具栏上选择 **Type** 工具。在工具顶部选项栏上将颜色设置为白色并选择你喜欢的任何字体。点击红色矩形并输入 Read More。会添加一个新的文本图层，位于矩形形状图层之上。确保 **Read More** 居中在矩形上方。

6. 现在我们需要为 **Over** 和 **Down** 做类似的设置，但与其重复整个过程，我们只需复制我们为 **Up** 状态制作的组结构。

7. 右键点击 **Up** 文件夹并选择 **Duplicate Group**。将新组命名为 Over 并点击 **OK**。

8. 新的 **Over** 组包含之前制作的图层的精确副本。我们将对文本图层中文字的颜色进行更改。

9. 再次选择 **Type** 工具并点击 **Read More copy** 图层。现在在屏幕顶部的工具选项中，将文字颜色更改为黑色。

10. 现在让我们再创建一个组。选择你最初创建的 **Up** 组，然后右键点击它并选择 **Duplicate Group**。将新组命名为 Down 并点击 **OK**。

11. 再次你拥有了 **Up** 组的精确副本。这次我们将文字保持为白色，但我们将添加一个图层效果，使按钮看起来像被按下。

12. 在 **Down** 组中选择 **Shape 1 copy** 图层。点击图层面板底部的 **fx**（图层样式）图标。从下拉菜单中选择 **Inner Shadow**。

13. 在 Inner Shadow 对话框中，将 Distance 设置为 2 px，Size 设置为 2 px。按钮上会出现一个小的暗影。如果我们想改变阴影的方向，拖动 Angle 按钮进行圆形移动。点击 OK。

14. 所以在所有这些之后，我们的图层和按钮应该看起来与以下屏幕截图相似：

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_224_0.png)

15. 我们必须合并每个组中的图层，以便每个组只包含一个图层。从顶部组（Down）开始。点击文本图层然后 Shift + 点击形状图层以便两个图层都被选中。右键点击并从下拉菜单中选择 Merge Layers。我们会看到我们的两个图层已变成一个图层。

16. 对 Over 组和 Up 组重复此过程，以便每个组只包含一个合并的图层。

17. 对于每个合并的图层，返回并重命名图层为 Up、Over 和 Down，如后面的屏幕截图所示。





18. 最后，我们不需要白色背景图层。点击 Background 图层将其选中，右键点击并选择 Delete Layer，或者直接按下键盘上的 Delete/Backspace 键。我们的最终图层设置应与以下截图类似：

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_225_0.png)

19. 将文件与你为网站使用的其他图像一起保存。将其命名为 ReadMore.psd。

## Placing the Photoshop button

现在我们已经完成了准备分层文件的繁重工作，就可以将该按钮图形放置到页面上了。你可以通过以下步骤操作：

- 1. 在 Muse 中，在 Design 视图中打开你想要放置按钮的页面。
- 2. 选择 File | Place Photoshop Button。Place Photoshop Button 对话框出现。浏览找到你的 Photoshop 文件 ReadMore.psd。点击 Select。
- 3. Photoshop Import Options 对话框出现。这让我们可以看到 Normal State、Rollover State、Mouse Down State 和 Active State 的预览（或在网页设计中常被称为 Up、Over、Down 和 Active）。

使用下拉菜单，我们可以指定 Photoshop 文件中的哪个图层将显示为按钮的 Normal State（或 Up 状态）、Rollover State（或 Over 状态）以及 Mouse Down State（或 Down 状态）。三个状态菜单会显示 Photoshop 图层的名称，并且缩略图会直观地显示每个所选图层的外观。你可以使用以下选项：

- - 对于 Normal State，从下拉菜单中选择 Composite
- - 对于 Rollover State，选择 Over 图层
- - 对于 Mouse Down State，选择 Down 图层

由于我们只为这三种状态设置了图层，因此可以将 Active 状态保留为 None。

4. 你可能会发现 Muse 已经为你正确选择了图层，根本无需更改菜单设置。点击 OK 以接受默认排列的状态。

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_226_0.png)

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_227_0.png)

在本示例中，我们使用非常简单的图形为一个外观相当标准的按钮创建了三种不同的状态。然而，在创建按钮时，你的想象力是唯一限制。它们甚至不一定非得看起来像按钮。一些设计师会为每个状态使用许多图层创建精美的设计。需要记住的关键是，在最终放置到 Muse 之前，先将每个组中的图层合并为一个图层。

5. 单击一次将 Photoshop 按钮放置到页面上。
与 Muse 中的大多数对象一样，我们可以根据需要缩放、旋转和定位新的 Photoshop 按钮。

6. 点击 Preview 链接来测试按钮。页面首次加载时，按钮的 Normal State 会出现。当我们将光标悬停在按钮上时，Read More 文本会变为黑色，当我们点击按钮时，文本会变回白色，并且按钮上会出现内阴影。我们可以按住按钮任意长时间，它都会保持这种外观。

## Adding a Photoshop image that's not a button

大多数设计师使用 Photoshop 不仅仅是创建按钮而已。在创建图像时，Photoshop 几乎只受我们想象力的限制。当我们在第 7 章 Working with Images 中讨论添加图像时，我们讨论了可在 Web 上使用的三种主要图像格式，即 JPEG、GIF 和 PNG。那么分层 Photoshop 文件（PSD）又如何融入其中呢？

答案是 Muse 让我们可以将多层 PSD 引入页面中，并在保存时创建该文件的一个适合在 Web 上显示的文件格式版本。最好还是让 Photoshop 将其转换为 JPEG、GIF 或 PNG，因为它在图像压缩方面做得更好，但让我们看看如何将 PSD 文件引入 Muse：

- 1. 在 Design 视图中打开你的页面。
- 2. 点击 File | Place。
- 3. 浏览到你的 PSD 文件所在的文件夹，选择它并点击 Open。

4. Image Import Options 框打开。你可以选择将图像作为 Composite Image 导入，或导入单个 Layer。如果选择 Composite，你将导入分层文件的扁平化版本。如果选择单个图层，你可以仅在透明背景上选择一个图层。选择 Clip To Layer Contents 意味着图像将被裁剪以去除该图层上对象周围的额外空间。

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_228_0.png)

5. 点击 OK，然后将图像定位到页面上的所需位置。同样，你可以根据需要缩放和旋转图像。

## Editing our Photoshop file

Muse 在处理 PSD 文件时提供的一个不错功能是，我们可以轻松返回并对 PSD 进行更改，它将在 Muse 网页中自动更新。使用以下步骤操作：

- 1. 在 Assets 面板中，你会看到新图像列为 YourFilename.psd。在面板中右键点击其名称以查看菜单选项列表。

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_229_0.png)

- 2. 如果你需要返回并编辑原始 PSD 文件，请选择 Edit Original。这会带你返回 Photoshop，你可以随意进行尽可能多的更改。
- 3. 在 Photoshop 中完成编辑后，选择 File | Save。
- 4. 返回 Muse，你会看到对 PSD 所做的更改会立即反映出来。

> 如果你的更改未出现，再次右键点击 Assets 面板中的文件名并选择 Update Asset。你现在应该能看到更新生效。

## Paste an image from another program

假设我们在 Photoshop、Illustrator 或 Fireworks 中创建了其他图像，并且想要将其中一个图像的全部或部分直接带入 Muse。操作方法如下：

- 1. 在另一个程序中，例如 Adobe Illustrator，通过选择其全部或部分，然后在键盘上选择 Ctrl + C（Windows）或 Command + C（Mac OS）来复制图像。在本示例中，我们复制了一个使用 Illustrator 中许多路径创建的图像。

![](img/8fa9b5d33ed7ba830d1ac29ef24477e4_230_0.png)

- 2. 在 Muse 的 Design 视图中打开我们想要放置图像的网页，并通过选择 Edit | Paste，或使用键盘上的 Ctrl + V（Windows）或 Command + V（Mac OS）来粘贴图像。
- 
- 就这样，图像出现在页面上。我们可以为图像添加描边或链接，以及根据需要进行缩放、旋转或定位。
- 3. 查看 Assets 面板。注意现在其中列出了一个对象为 clipboard。这是我们刚刚从其他程序粘贴的文件。当我们保存网站时，Muse 会将此资源作为网站的一部分保存。

## Embedding rich media content

如果我们有一些富媒体，例如需要添加到网页中的 Flash 演示文稿，我们可以像添加图像一样轻松地完成此操作。请按照以下步骤操作：

- 1. 在 Design 视图中打开你想要添加媒体文件的页面。
- 2. 选择 File | Place。在 Import 对话框中，导航到包含媒体文件的文件夹并选择所需的文件。Muse 支持 Adobe Flash SWF 文件。
- 3. 点击 OK 并将对象放置到页面上的所需位置。

如果我们想向页面添加视频文件，强烈建议将视频上传到 YouTube 等服务。YouTube 主机托管视频，因此我们不必担心网站的带宽问题。我们只需按照第 9 章 More Widgets – Compositions and Slideshows 中概述的方式，将 YouTube 提供的任意 HTML 代码添加到页面中即可。

## Summary

在本章中，我们简要研究了如何在 Photoshop 中创建分层图像，然后将其作为 Photoshop 按钮放置在 Muse 中。这使我们能够创建具有多种状态的按钮，这是一种向网站访问者提供反馈的有用方式。我们还看到了将另一个程序中创建的图像复制并粘贴到 Muse 中是多么容易。

在下一章中，我们将研究在发布网站之前预览和测试整个网站。




