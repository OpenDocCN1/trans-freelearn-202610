
## 使用的约定

本书通篇使用了多种文本约定。

CodeInText: 表示正文中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、示例 URL、用户输入和 X 账号。例如："现在按 Ctrl + S 将场景保存为 player.tscn 到我们的 Scenes 文件夹中。"

代码块如下所示：

```
extends Node2D

@onready var update_health_hud = $HUD
@onready var player = $Player
@onready var strawberries = $Collectibles

func _ready():
    player.level_strawberries = strawberries.get_child_count()

func _process(delta):
    update_health_hud.frame = player.get_hearts()
```

当我们希望在步骤中引起您对命令行输入或输出的注意时，相关行或项目会以粗体显示：

```
extends Node2D
var health : int = 100
var jump_strength : float = 220.5
var is_dashing : bool = true
var player_name : String = "Riptide"
var player_level : int = 23
var player_exp : float = 123.45
var has_key : bool = false
var spike_damage : int = 10
```

在 Linux 上，在终端中运行以下命令：

```
Unzip Godot_v4.3-stable_linux.x86_64.zip -d Godot
```

**粗体：** 表示新术语、重要词语，或您在屏幕上看到的词语。例如，菜单或对话框中的词语会以这种方式出现在文本中。例如：在 **Particle Flags** 设置中启用 **Rotate Y** 并禁用 **Disable Z**。

警告或重要说明如下所示。

技巧和窍门如下所示。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果您对本书的任何方面有疑问或有任何一般性反馈，请发送电子邮件至 customercare@packt.com，并在邮件主题中提及本书的标题。

**勘误：** 虽然我们已尽一切努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 http://www.packt.com/submit-errata，点击 **提交勘误**，并填写表单。

**盗版：** 如果您在互联网上发现我们作品的任何形式的非法复制，我们将不胜感激您提供位置地址或网站名称。请通过链接至该材料联系我们 copyright@packt.com。

**如果您有兴趣成为作者：** 如果您在某个主题方面有专长，并且有兴趣撰写或为一本书做出贡献，请访问 http://authors.packt.com/。

## 分享您的想法

在阅读完 *Godot 4 for Beginners* 后，我们很想听听您的想法！请点击此处直接前往本书在亚马逊的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，并将帮助我们确保提供优质的内容。

## 加入我们的 Discord 社区

加入我们的社区 Discord 空间，与作者和其他读者进行讨论：

```
https://packt.link/godot-4-game-dev
```

![](img/d34207e07c133a9e84d9caf1d479006c_23_0.png)

## 第 1 部分

### 学习 Godot 引擎的工作原理

在本书的第一部分中，您将建立使用 Godot 游戏引擎的坚实基础。我们将首先探索 Godot 的起源以及使其成为游戏开发独特而强大的工具的原因。然后，您将亲手操作引擎的界面，了解场景、节点和编辑器如何协同工作。您还将迈出进入 3D 游戏开发的第一步，并使用 GDScript 进行脚本编写，这是一种直观、类似 Python 的 Godot 语言。到本部分结束时，您将准备好自信地开始创建交互式游戏。

本书的这一部分包含以下章节：

- *第 1 章，开始使用 Godot！*
- *第 2 章，探索 Godot 引擎界面*
- *第 3 章，3D 简介*
- *第 4 章，使用 GDScript 编程*

## 1

### 开始使用 Godot！

在本章中，我们将探索 Godot 4.0 的历史和核心功能，这是一款以多功能性和易用性著称的游戏开发引擎。无论您是有抱负的独立开发者还是寻求新工具的资深创作者，了解 Godot 的独特性都将为您的游戏开发之旅奠定基础。

本章有两个主要目的。首先是向您介绍 Godot 的背景和关键功能，展示其为何成为游戏开发社区中备受青睐的选择。其次是指导您设置 Godot 4.0 并创建第一个项目，确保您拥有开始制作自己的游戏所需的工具和信心。

在本章中，我们将涵盖以下主要主题：

- Godot 是什么？
- 设置 Godot 4
- 创建新项目

到本章结束时，您不仅将掌握使 Godot 脱颖而出的基本功能，还将具备在引擎中安装、启动和创建第一个项目的知识。这些是为我们在后续章节中完成的一切奠定基础的基本步骤。

## 充分利用本书——了解您的免费福利

解锁随购买而来的独家免费福利，经过精心设计，可助您学习之旅更上一层楼，帮助您无限制地学习。

以下是您通过本书获得的内容的快速概览：

### 下一代阅读器

![](img/d34207e07c133a9e84d9caf1d479006c_28_0.png)

图 1.1：下一代 Packt Reader 功能的插图

我们的基于网络的阅读器旨在帮助您有效学习，具有以下功能：

- 多设备进度同步：从任何设备学习，进度同步无缝。
- 高亮和笔记：让您的阅读转化为持久的知识。
- 书签：随时重温您最重要的学习内容。
- 深色模式：通过切换到深色或棕褐色模式，专注于学习并减少眼部疲劳。

### 交互式 AI 助手（测试版）

![](img/d34207e07c133a9e84d9caf1d479006c_29_0.png)

图 1.2：Packt AI 助手的插图

我们的交互式 AI 助手已在本书内容上进行训练，以最大化您的学习体验。它具有以下功能：

- ✨ 总结它：总结关键章节或整章内容。
- ✨ AI 代码解释器：在下一代 Packt Reader 中，点击每个代码块上方的 Explain 按钮即可获得 AI 驱动的代码解释。

注意：AI 助手是下一代 Packt Reader 的一部分，仍处于测试阶段。

### 无 DRM 的 PDF 或 ePub 版本

![](img/d34207e07c133a9e84d9caf1d479006c_29_1.png)

图 1.3：免费 PDF 和 ePub

通过购买包含的以下特权，无限制地学习：

- 📄 使用本书的无 DRM PDF 副本，随时随地学习。
- 📄 使用您最喜欢的电子阅读器，使用本书的无 DRM ePub 版本进行学习。

## 立即解锁本书的独家福利

扫描此二维码或访问 packtpub.com/unlock，然后按名称搜索本书。确保是正确的版本。

注意：在开始之前请准备好您的购买发票。

![](img/d34207e07c133a9e84d9caf1d479006c_30_0.png)

![](img/d34207e07c133a9e84d9caf1d479006c_30_1.png)

## 技术要求

您可以在 GitHub 仓库中找到本书的示例项目和代码：https://github.com/PacktPublishing/Godot-4-for-Beginners

本章的代码文件可在以下位置获取：https://github.com/PacktPublishing/Godot-4-for-Beginners/tree/main/ch1/godotintro

访问此链接查看代码运行的视频：https://packt.link/RjbW3

### Godot 是什么？

简短的回答是 Godot 是一个游戏引擎。它最初由 Juan Linietsky 和 Ariel Manzur 于 2007 年创建，作为阿根廷多个工作室的内部工具。您接下来的问题可能是，“游戏引擎到底是什么？”以及“Godot 有什么特别之处？”

制作游戏很难——真的很难！有无数个 moving parts 都需要完美协同工作。虽然可以从零开始构建一切，但大多数开发者依赖游戏引擎来处理繁重的工作。

因此，游戏引擎是一个简化游戏开发的软件框架。它提供构建游戏的核心工具和系统——例如渲染、物理、输入处理和资源管理——以便开发者可以更多地关注游戏玩法、故事和设计，而不是低级技术代码。
