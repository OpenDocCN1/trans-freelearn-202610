

BK-Thumb 可以自动拾取多张图片，添加标题、标题等。它包含了一些很棒的插件功能。我建议你试用一下，看看是否适合你的项目。你可能会注意到 BK-Thumb 也会影响网站上的其他一些图片，因此这种解决方案适合某些人而不适合其他人。

## 使用 RokBox 进行多媒体显示

我之前提到过，我最喜欢的扩展之一是 Rocket Theme 的 RokBox；这是因为它能够以多种方式显示多种类型的内容。RokBox 是一个基于 MooTools 的 JavaScript 幻灯片，允许你快速轻松地显示多种媒体格式，包括图片、视频（也包括视频分享服务）以及音乐。以下是如何在你的网站上轻松使用它的方法：

- 1. 在 http://extensions.joomla.org 的 Joomla! 扩展区域搜索 "RokBox"。
- 2. 访问扩展开发者的网站并将扩展下载到你的电脑。 （你也可以安装内容按钮，但我们确实需要主 rokbox-system.zip 文件）。
- 3. 使用扩展 | 安装 / 卸载 功能在 Joomla! 管理后台上传此扩展。
- 4. 在管理后台导航至 扩展 | 插件管理器，并启用 "RokBox" 插件：

![](img/0151410972a5c5ea0aab26416e44b1da_315_0.png)

RokBox 插件是通过在内容项和模块中使用特殊标签或链接代码触发的。开发者的网站包含一个有用的文档页面和论坛。你可以在 http://www.rockettheme.com/extensions-joomla/rokbox 找到主要的 RokBox 页面。

RokBox 的工作原理非常简单。所有潜在的 RokBox 链接只需要在其内部添加一个额外的元素即可与任何超链接配合使用：<a>。此元素用于将以下代码放入你的链接中：<a rel="rokbox" href="/...">example</a>。当 RokBox 找到包含此信息的超链接时，RokBox 会将其纳入考虑。使用这种方法意味着你可以在模块中使用 RokBox，而不仅仅是在文章中使用。如果你确实想在 Joomla! 文章中使用 RokBox，那么你可以使用另一种方法（以下标签），RokBox 就是为识别这些标签而构建的：

```
{rokbox...}link to the file{/rokbox}
```

我们可以在这个基本标签中添加许多其他参数，以开始为你的媒体显示标题和其他信息：

```
{rokbox size=|100 50| text=|my rokbox| thumb=|images/thumb.jpg| title=|Head :: Text| album=|photos| module=|login|}images/image.jpg{/rokbox}
```

你引用媒体的方式由你决定。更多信息可以在 RokBox 文档中找到。

现在，让我们通过导航到 扩展 | 模块管理器 屏幕，点击 "新建" 图标，然后点击 "自定义 HTML" 来创建一个新的自定义 HTML 模块，以创建一个新的自定义 HTML 模块。

给模块一个名为 "RokBox" 的标题，并确保位置设置为 "Left"。然后我们需要在该屏幕的 "自定义输出" 区域输入以下内容：

```
<a href="images/stories/image1.jpg" title="Image 1 :: Image Description" rel="rokbox[600 400]">Link to RokBox Image 1</a>
```

之前的代码为名为 Link to RokBox Image 1 的文本添加了一个超链接，给它一个标题 Image 1 和描述 Image Description，并在 600 x 400 像素的窗口大小中使用 RokBox 打开图像。确保你在 /images/stories 文件夹中有名为正确名称的图片，然后点击屏幕右上角的 应用 按钮以应用你的更改。

当你查看网站的前端时，你现在应该有一个指向 RokBox 功能的文本链接：

![](img/0151410972a5c5ea0aab26416e44b1da_316_0.png)

RokBox 可以像我们在这里使用的一样使用，或者你也可以在 RokBox 链接周围插入一张图片，从而有效地创建一个指向 RokBox 内容的缩略图链接。许多选项都是可能的，包括使用 RokBox 创建带缩略图的图像画廊。

## 添加视频功能

视频是当下的媒体元素！如本书前面所强调的那样，现在有许多很棒的视频功能可用于 Joomla!，因此让我们继续并将一些功能集成到我们的项目中。

## RokBox，再来一次！

由于 RokBox 插件也可以播放视频，让我们在 RokBox 侧边模块中添加内容，以在我们最近创建的 RokBox 图像链接下方显示一个 YouTube 视频。以下是步骤：

- 1. 在 扩展 | 模块管理器 中打开 RokBox 模块。
- 2. 将以下代码添加到自定义输出框中：

```
<p>
<a href="http://www.youtube.com/watch?v=6RWQ2OQ7_2Y"
 title="Cars :: 2010 Aston Martin Vantage V 12"
 rel="rokbox(425 373) (demo)">Link to RokBox YouTube Video</a>
</p>
```

代码的分解如下：

- * 链接指向一个 YouTube 主机视频。
- * 标题是 Cars。
- * 描述是 2010 Aston Martin Vantage V 12。
- * 它将在 425 px x 373 px 大小的 RokBox 窗口中打开。
- * 文本是 Link to RokBox YouTube Video。我在周围放置了段落标签，以便与我们之前创建的图像链接创建间距。

点击页面右上角的 应用 以保存更改。

可以使用 RokBox 在模块和文章中显示视频画廊。我认为你会同意这是一个很棒的多媒体扩展！

## 使用 hwdVideoShare 进行视频

如果你想要一个像 YouTube 那样的强大的视频方法，hwdVideoShare 视频分享画廊是一个开源（GNU GPL）视频分享 Joomla! 扩展，其功能类似于 YouTube 等网站。它具有多种上传工具，可用于大媒体上传。该组件支持众多视频格式。

让我们看看如何为我们的项目设置它：

- 1. 在 http://extensions.joomla.org 的 Joomla! 扩展区域搜索 "hwdVideoShare"。
- 2. 访问扩展开发者的网站并将扩展下载到你的电脑。
- 3. 在上传到 Joomla! 之前先解压下载的文件。压缩包包含 ZIP 文件中的众多扩展。
- 4. 使用 扩展 | 安装 / 卸载 功能将 com_hwdVideoShare.gz 文件上传到 Joomla! 管理后台。在管理后台导航至 组件菜单 | hwdVideoShare。在此页面上，你可以选择安装示例视频数据。同意非商业许可并点击该页面上的 "完成设置" 图标：

![](img/0151410972a5c5ea0aab26416e44b1da_319_0.png)

设置完成后，你将看到一个包含清理选项和对视频组件进行维护的页面。保持所有设置不变并点击屏幕右上角的 "运行工具" 图标。维护应该会运行并刷新屏幕。

现在让我们看看在安装了主组件和一些默认数据后我们有什么。在管理后台导航至 菜单 | 主菜单 并点击屏幕右上角的新建以创建新的菜单项。从可用列表中选择 hwdVideoShare，然后从子列表中选择 视频主页：

![](img/0151410972a5c5ea0aab26416e44b1da_320_0.png)

给菜单链接一个名为 "Video Gallery" 的标题并点击屏幕右上角的 保存。如果现在查看网站的前端，你将看到一个指向 hwdVideoShare 组件的新菜单项：

![](img/0151410972a5c5ea0aab26416e44b1da_320_1.png)

hwdVideoShare 组件非常庞大，充满了作者演示站点上显示的视频功能。可以创建一个像 YouTube 一样的完整视频托管服务（在你的 Joomla! 站点内），也可以简单地很好地分类视频并锁定前端上传功能。我很想花更多时间填充此组件并添加视频功能，但我认为支持文档解释了此扩展的所有功能。现在，我们需要继续并为我们的项目添加更多多媒体功能。

## 使用 FlowPlayer 在文章中嵌入视频

对于大多数人来说，能够在文章中放置视频可能是一个你经常需要的技术。FlowPlayer 使用基于 Flash 的视频播放器，通过使用插件标签轻松在文章中播放视频文件：

```
{flowplayer}URL_Video{/flowplayer}
```

FlowPlayer 可作为 Joomla! 插件使用，以便添加到你的文章中，也可作为模块放置在你需要的任何模块位置。要将 FlowPlayer 添加到 Joomla!：

- 1. 在 http://extensions.joomla.org 的 Joomla! 扩展区域搜索 "FlowPlayer"。
- 2. 访问扩展开发者的网站并将扩展下载到你的电脑。
- 3. 使用 扩展 | 安装 / 卸载 功能将视频和插件扩展都上传到 Joomla!。
- 4. 导航至 扩展 | 插件管理器 并发布 内容 – FlowPlayer 插件：

![](img/0151410972a5c5ea0aab26416e44b1da_321_0.png)

> 还有另一个名为 Flow Player 的插件，是为与 hwdVideoShare 组件一起使用而安装的。确保你启用的是 内容 – FlowPlayer 插件，即我们刚刚安装的那个。

