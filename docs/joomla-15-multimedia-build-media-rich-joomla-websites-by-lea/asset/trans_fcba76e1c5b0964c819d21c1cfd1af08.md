

如果我在 JMultimedia 项目主页上运行本地验证，我会收到该页面 50 个错误和 33 个警告。现在如果我试图在网站上线前使其通过验证，就还有一些工作要做。这些错误都可能导致网站在各种情况下无法使用，因此我必须努力减少主要错误。

![](img/0151410972a5c5ea0aab26416e44b1da_350_0.png)

在对这些验证错误进行快速评估后，似乎有一两个扩展被使用，导致了大部分这些错误。我们现在的选项是：

- 尝试修复导致错误的原因。
- 卸载扩展并寻找其他解决方案。
- 忽略这些然后去泡杯茶（我不推荐这一项，虽然喝杯茶也没什么不好）。

解决验证错误的一个很好的开始方式是确保你的模板通过验证。你可以通过创建一个新的 Joomla! 文章，并在其中放置一个简单的 `<p></p>` 标签来检查这一点，保存它，并为其创建一个菜单链接。在前端访问该页面。接下来，确保该页面上没有加载任何模块，然后使用浏览器验证工具，例如 Mozilla Firefox 的“Web Developer Toolbar”，或在线验证站点，例如 `http://validator.w3.org` 来验证它。

结果将突出模板结构的基础以及其结构方式是否存在任何问题。从这里开始，你可以开始引入页面元素并轻松找到罪魁祸首，或检查验证工具结果以准确找到问题所在。

## 收尾

如果你在本地开发了你的项目，那么下一步将是将其移至远程 Web 服务器。这通常通过 FTP 或 shell 命令执行。Joomla! 帮助网站上有一些关于此过程的优秀文章。

按照 Joomla! 的惯例，甚至还有许多扩展可以帮助此过程，其中值得一提的是 JoomlaPack。JoomlaPack 是一款网站备份实用程序，使你能够复制你的网站和数据库，并在任何兼容的 Joomla! 服务器上重新安装。

JoomlaPack 不仅允许你将网站从本地开发环境移至远程服务器，而且还为你提供了随时需要的简易网站备份功能。你可以通过在 http://extensions.joomla.org 搜索 JoomlaPack 来找到 JoomlaPack。

祝你的网络项目好运。

## 摘要

本章采用了 cookbook 风格的方法，取了一个正在运行的 Joomla! 副本并增强了框架，使其包含一系列多媒体功能。

我们重点介绍了设置本地开发环境以创建安全且易于使用的开发区域，在此我们可以评估扩展并开发我们的项目。

安装了一系列扩展并在我们的工作副本中配置了 Joomla! 功能，突出了 Joomla! 可以轻松扩展为媒体丰富的网站的方式。

最后，我们通过一些可访问性测试收尾，并强调了将网站移至其最终驻地的过程。

## A

## 扩展类型及如何安装它们

为了充分利用 Joomla! 可以提供的全部功能，你在某个阶段需要向你的 Joomla! 包安装扩展。感谢扩展的设计以及 Joomla! 中包含的扩展管理器工具，安装 Joomla! 扩展是一项轻松的任务：

![](img/0151410972a5c5ea0aab26416e44b1da_353_0.png)

> 请务必在向你的 Joomla! 网站安装任何第三方扩展之前备份你的文件和数据库。

## 扩展类型

**扩展**一词是通用术语，用于描述用于扩展你的 Joomla! 网站的五大类“可扩展元素”。这些是：

## 插件

**插件**是功能强大的扩展，可为你的网站添加额外功能。插件可以在不同层面上工作，例如内容、搜索或系统范围级别：

![](img/0151410972a5c5ea0aab26416e44b1da_354_0.png)

插件的一个例子可能是向你的内容文章中添加一个简单的标签，以便轻松嵌入电影而无需复杂的 HTML 代码，或增强 Joomla! 搜索系统以便同时搜索包含的扩展。

你可以通过 **扩展管理器 | 插件管理器** 菜单链接管理你的插件。

## 组件

**组件**可以为你的 Joomla! 网站添加全新的功能，它们通常可以被归类为迷你应用程序：

![](img/0151410972a5c5ea0aab26416e44b1da_355_0.png)

组件为你的网站添加了“购物车”和“目录列表”等出色功能，并且一旦安装就可以在组件菜单下找到。组件通常包含各种元素，并包含指向配置和管理屏幕的附加子菜单链接。

## 模块

模块是帮助向最终用户传递页面信息的扩展：

![](img/0151410972a5c5ea0aab26416e44b1da_355_1.png)

它们可以从简单的 HTML 输出到系统提供的复杂信息，并在你的网站页面上显示这些信息。模块通常与组件相关联，因为它们为网页提供信息。模块的一个例子可以是简单的天气预报、投票问题，或由事件组件生成的事件列表。

模块在 扩展 | 模块管理器 区域中管理。

## 模板

Joomla! 模板影响你网页的整体设计和结构：

![](img/0151410972a5c5ea0aab26416e44b1da_356_0.png)

你可以通过使用 Joomla! 模板轻松为你的数据库驱动网站创建完全不同的风格。模板包含模块位置，有助于组织和为已发布的模块提供结构。

模板通常包含许多调整参数，这些参数在 Joomla! 管理中的模板管理器中作为模板选项提供。

## 语言

语言是 Joomla! 的核心功能，这些包可将整个 Joomla! 界面（包括你的 Joomla! 网站的前端和管理）翻译出来：

![](img/0151410972a5c5ea0aab26416e44b1da_356_1.png)

语言在 扩展 | 语言管理器 区域中管理。

## 如何安装

安装扩展的最简单方法之一是下载扩展并在你的计算机上保留一份副本。除了保留原始扩展文件的副本外，扩展已编写为可通过扩展管理器轻松安装，所有工作由扩展管理器完成：

![](img/0151410972a5c5ea0aab26416e44b1da_357_0.png)

登录你的 Joomla! 管理并导航到 **扩展管理器** 菜单项。

在 **上传安装包文件** 部分，单击 **选择文件** 按钮以在本地计算机上导航以找到你想要上传的扩展。选择后，单击 **上传文件并安装** 按钮：

![](img/0151410972a5c5ea0aab26416e44b1da_357_1.png)

你应该在网站上收到 **上传成功** 消息。你的扩展可能要求你去配置一些参数，或发布作为扩展包安装的一部分的插件和模块。

## 如何卸载

Joomla! 1.5 的扩展可以通过单击扩展管理器页面上的 **管理** 菜单项轻松卸载。

在要卸载的扩展旁边的复选框中放置一个勾选标记，然后单击页面右上角的 **卸载** 图标。

![](img/0151410972a5c5ea0aab26416e44b1da_357_2.png)

你的扩展将被卸载，屏幕上会显示一条确认消息。

## 索引

A
accessibility, Joomla!
  about 334
  general considerations 335
accessibility, Joomla! Templates
  about 256
  accessible content 256
  accessible template code 256
  colors 257
  contrasts 257
  fonts 257
Adobe Flash 163
Ajax Whois extension
  about 219
  download link 220
  features 220
AllVideos
  about 176
  features 177
alternative content 152
Apache server 263
Apple QuickTime
  about 163
Apple QuickTime Player 184
audio
  about 115, 116
  and, internet 116, 117
  browser support 151
  compressing 121, 122
  encoding 121, 122
  including, Joomla! Plugin used 131-133
audio, Joomla! features
  adding 311
  playing, RokBox used 314
  podcast, adding 315, 318
audio, multimedia in Joomla! CMS 17, 18
audio accessibility 151
audio codecs 118
audio compression 118
audio content, Joomla! Articles
  article, creating 127
  audio HTML code 127
  editors 125
  editor settings 126
  including, Joomla! plugin 131-133
  source 123
  source material, encoding 123
  uploading 123
  uploading, via FTP 123
  uploading, via Media Manager 124, 125
  WYSIWYG editors 125
audio controls 152
audio extensions, Joomla!
  about 312
  MP3 Browser 312
  RokBox 314
  Simple MP3 Player 313
audio file format
  AAC 119
  about 118
  AIFF 119
  MIDI 119
  MP3 119
  RealAudio 119
  WAV 119
  WMA 119
audio formats
  about 117
  audio codecs 118
  audio compression 118

