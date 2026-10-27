
WebStorm 拥有许多强大的功能，可帮助我们编写 Web 应用程序：

-   代码自动补全：在键入时列出属性和方法
-   静态分析器：在键入时扫描可能存在的问题
-   重构工具：帮助重命名实体和提取方法
-   快速修复：轻松纠正常见错误
-   通过代码树导航并查找函数用法
-   代码格式化程序：保持源代码整洁
-   强大的调试器

## 替代开发环境

如果你希望使用其他 IDE，IntelliJ IDEA 和 Eclipse 都有相应插件。如果你更喜欢简单的文本编辑器，Sublime、Emacs 和 Vim 也有插件。这些都可以在 DartLang 下载页面上找到。

如果你是 Visual Studio 的忠实用户，目前有一个社区项目正在将 Dart 引入该环境。DartVS 可以从 Visual Studio Gallery 下载。它直接使用 SDK 中的 DartAnalyzer 在 IDE 内报告错误、警告和提示。IntelliSense 和其他功能的支持也正在进行中。

对于喜欢轻量级编辑器而非完整开发环境的人来说，Atom 编辑器（https://atom.io/）是一个选择。名为 dartlang（https://atom.io/packages/dartlang）的插件提供了代码补全、语法高亮以及访问一些 pub 命令的功能。

## 帮助启动项目

为了帮助你创建新项目，Dart 提供了一个名为 Stagehand 的工具，它可以根据你选择的高质量项目模板创建脚手架。它处理常见任务，如文件夹结构、公共库和样式表。它甚至有自己的网站（http://stagehand.pub/）。

在 WebStorm 中，创建新项目时会提供一系列强大的选项，允许你设置 Dart SDK 的位置，并为示例内容选择项目类型。项目类型提供一系列骨架应用，由 SDK 自带的 Stagehand 应用驱动：

![](img/52c05b53a56abd53584e4fed738ed509_29_0.png)

Stagehand 也可以在命令行中运行，第一步是将其安装为命令：

```
pub global activate stagehand
```

此步骤从 stagehand 包中安装 stagehand 命令：

```
mkdir aproject
cd aproject
stagehand web-full
```

这将基于 web-full 模板创建一个包含示例内容的文件夹。

> 有关当前模板的完整详细信息，请参阅
>
> https://pub.dartlang.org/packages/stagehand.

## SDK 中的其他内容

下载内容还包括一组命令行工具，位于 dart-sdk/bin 路径下。

| 工具 | 描述 |
|---|---|
| dart | 独立的 Dart 虚拟机本身。 |
| dart2js | 将 Dart 转换为 JavaScript 以便在任何现代浏览器上运行的工具。 |
| Dartanalyzer | 分析 Dart 源代码中的错误和建议。开发环境也使用它向用户提供反馈。 |
| Dartdocgen | 直接对 Dart 代码工作的文档生成器。 |
| Dartfmt | 智能代码格式化程序，让你的代码整洁一致。 |
| pub | 包管理和应用程序部署工具。 |

> 你可能希望将这些工具添加到系统 PATH 中，以便可以从命令行的任何位置调用它们。这是可选的，因为 WebStorm 为这些工具提供了简单的界面；然而，许多开发者认为这对良好的工作流程至关重要。
>
> 有关 pub 的安装和配置的更多详细信息，请参阅 https://www.dartlang.org/tools/pub/installing.html

此位置还有一个 snapshots 文件夹。大多数工具本身是用 Dart 编写的；当它们启动并初始化时，可以将初始化状态的二进制映像存储起来，用于将来的程序执行。

这些快照是使用 Dart 可执行文件生成的，主要面向部署中经常使用的命令行应用程序。

## 构建你的第一个应用程序

从简单开始是个好主意，但我们也希望从一些有用的东西开始，随着我们探索 Dart 可以扩展。

在日常软件开发中，文本编辑器往往是巨大、改变世界的软件的起点。我们将构建一个基于 Web 的多用途文本编辑器，用作代码片段、待办事项列表或去除复制文本格式的便签——非常方便。为了增加真实感，过程中还会有一个虚构的客户要求新功能。

在下一章中，我们将以此为基础，构建一些更华丽的功能和图形显示。

> **下载示例代码**
>
> 你可以从你的账户下载所有已购买的 Packt 图书的示例代码文件，网址为 `http://www.packtpub.com`。如果你在其他地方购买了这本书，可以访问 `http://www.packtpub.com/support` 并注册，以便将文件直接通过电子邮件发送给你。

### 探索 Web 项目结构

进入 WebStorm 欢迎页面，点击 **打开**。选择你解压示例代码的文件夹，然后选择 `Chapter One` 文件夹，再选择名为 `Start` 的子文件夹。

打开任何 Dart 项目时，一个好的第一步是确保所有依赖项都存在且是最新的。为此，选择 `pubspec.yaml` 并右键单击以显示上下文菜单。点击 `Pub: Get Dependencies` 项。然后，依赖项将被解析并在需要时获取。

| 文件/文件夹 | 描述 |
|---|---|
| `packages` | 指向此项目正在使用的包（或库）的链接。在这种情况下，你会看到“browser”包。 |
| `pubspec.lock` | 控制正在使用的包的特定版本。 |
| `pubspec.yaml` | 我们项目的元信息（名称、描述等）以及正在使用的包列表。 |
| `web/styles/main.css` | 标准级联样式表文件。 |
| `web/index.html` | 我们 Web 应用程序的入口点。 |
| `main.dart` | 最后但同样重要的是，一个 Dart 文件！ |

### 解析包

从前面的列表可以看出，即使是一个简单的 Dart 项目，也有大量的包结构和元文件。该语言的哲学是库小而专注，并且可能来自不同的来源。它们数量众多，并且按不同的时间表更新，因此版本控制很重要——依赖项是贪婪的，必须满足它们。

### 启动文本编辑器

你希望你的团队成员能够毫无依赖噩梦地构建和使用你的新 Uber 包！同样，你希望从互联网上下载的包能够“直接运行”，并减少编写应用程序所需的工作量。

为了管理所有这些包，Dart 有一个名为 pub 的工具。这是一个瑞士军刀式的工具，帮助创建、管理和部署你的程序和包。Dart 包的主要家园是网站 https://pub.dartlang.org，那里发布了许多开源库。

> 💡 如果你好奇这个名字的起源，“pub”是英国俚语，指人们喝酒和玩飞镖游戏的酒吧。

### 了解 Pubspec

`pubspec.yaml` 有一个奇怪的文件扩展名——YAML 代表**另一种标记语言**。

```
name: 'TextEditor'
version: 0.0.1
description: A web based text editor.
environment:
  sdk: '>=1.0.0 <2.0.0'
dependencies:
  browser: any
```

由此可见，pubspec.yaml 相当于 Dart 的项目文件或解决方案文件。它包含典型的依赖项详细信息和元信息。Pubspec 文件的完整详细信息位于 https://www.dartlang.org/tools/pub/pubspec.html。虽然它是一个功能强大且内容丰富的文件，但你与它的主要交互可能只是添加新依赖项。

### 将 Dart 放入网页

文件 `index.html` 是一个普通的网页。文件的其余部分由几个简单的 `<div>` 元素组成，并且正如你可能期望的那样，还有一个 `<textarea>` 标签：

```
<!DOCTYPE html>
<html>
<head>
    <title>TextEditor</title>
    <link rel="stylesheet" href="styles/main.css">
</head>
```

