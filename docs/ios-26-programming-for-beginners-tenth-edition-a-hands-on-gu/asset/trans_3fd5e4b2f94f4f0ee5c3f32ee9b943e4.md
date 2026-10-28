

这就是使用 async-let 允许 `makeToast()` 和 `poachEgg()` 方法并行运行的原因，并且 `poachEgg()` 方法不再等待 `makeToast()` 方法完成后再开始执行。太棒了！

你已成功在你的应用中实现了异步代码。太棒了！关于 Swift 并发还有很多东西需要学习，例如结构化并发和 actors，但这超出了本章的范围。

给自己一个赞；你已完成了本书的第一部分！

## Summary

在本章中，你学习了 Swift 并发以及如何在 Breakfast-Maker 应用中实现它。

你首先学习了 Swift 并发的核心概念。然后，你检查了一个没有并发的应用并探索了其问题。之后，你打开了严格并发检查并使用 async/await 在应用中实现了并发。最后，你通过使用 async let 使你的应用更高效。

你现在了解了 Swift 并发的基础知识，将能够在自己的应用中使用 async/await 和 async-let。

在下一章中，你将通过使用 storyboards 创建其屏幕来开始编写你的第一个 iOS 应用程序，storyboards 允许你在无需编写大量代码的情况下快速原型化一个应用程序。

## Get This Book’s PDF Version and Exclusive Extras

扫描二维码（或访问 packtpub.com/unlock）。按书名搜索此书，确认版本，然后按照页面上的步骤操作。

Note: Keep your invoice handy. Purchases made directly from Packt don’t require an invoice.

![](img/0972ba626734a3986e095c788777818e_162_0.png)

## 10

## Set Up the User Interface

在本書的 *Part 1* 中，你学习了 Swift 语言及其工作原理。现在你对该语言有了良好的工作知识，可以学习如何开发 iOS 应用程序。在本部分中，你将构建期刊应用 *JRNL* 的 **用户界面 (UI)**。你将使用 Xcode 的 **Interface Builder** 来实现这一点，编码量将保持在最低限度。

你将从学习本書中广泛使用的 iOS 应用开发常用术语开始。接下来，你将参观 *JRNL* 应用中使用的屏幕并了解用户如何使用该应用。之后，你将使用 Interface Builder 开始重建应用的 UI，从允许用户在 Journal List 和 Map 屏幕之间选择的标签栏开始。最后，你将为两个屏幕顶部添加导航栏并配置标签栏按钮。

到本章结束时，你将学习 iOS 应用开发中常用的术语、你的应用流程会是什么样子，以及如何使用 Interface Builder 添加和配置 UI 元素。

本章将涵盖以下主题：

- iOS 开发中的有用术语
- *JRNL* 应用的概览
- 修改你的 Xcode 项目
- 设置标签栏控制器场景

## Technical requirements

你将修改你在 *Chapter 1, Hello, Xcode* 中创建的 JRNL Xcode 项目。

本章的资源文件和完成的 Xcode 项目位于本书代码包的 Chapter 10 文件夹中，可在此下载：

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

观看以下视频以查看代码的运行情况：

https://youtu.be/pof2hChUGBg

在开始项目之前，你将学习 iOS 开发中使用的一些常用术语。

## Useful terms in iOS development

当你开始使用 UIKit 进行 iOS 应用开发时，你会遇到特殊的术语和定义。以下是一些最常用的术语和定义。现在先通读它们。即使你现在还不完全理解一切，随着你的进展，一切都会变得更清晰：

- **View:** View 是 `UIView` 类或其子类的一个实例。你在屏幕上看到的任何东西（按钮、文本字段、标签等）都是一个 view。你将使用 views 来构建你的 UI。

> Classes are covered in *Chapter 7, Classes, Structures, and Enumerations.*

- **Stack view:** Stack view 是 `UIStackView` 类的一个实例，它是 `UIView` 的子类。它用于将 views 以水平或垂直堆栈的方式分组。这使得它们更容易使用 **Auto Layout** 在屏幕上定位，后者在本节后面讨论。

- **View controller:** View controller 是 `UIViewController` 类的一个实例。每个 view controller 都有一个 `view` 属性，其中包含对 view 的引用。它决定 view 向用户显示什么以及用户与 view 交互时发生什么。

> View controllers will be discussed in detail in *Chapter 14, MVC and Table Views.*

## Total retail sales of consumer goods

National Bureau of Statistics

![](img/0972ba626734a3986e095c788777818e_167_0.png)

- **Model-View-Controller (MVC)**: 这是 iOS 应用开发中非常常见的设计模式。用户与屏幕上的 views 交互。应用数据存储在数据模型对象中。控制器管理 views 与数据模型对象之间信息流。

> MVC will be discussed in detail in *Chapter 14, MVC and Table Views.*

- **Storyboard file**: Storyboard 文件包含用户所见内容的视觉表示。应用的每个屏幕都由一个 storyboard scene 表示。

打开你在 *Chapter 1, Hello, Xcode* 中创建的 JRNL 项目，然后点击 Main storyboard 文件。

![](img/0972ba626734a3986e095c788777818e_170_0.png)

你会在其中看到一个场景，并且当你在 **Simulator** 中运行你的应用时，该场景的内容将显示在屏幕上。Storyboard 文件中可以有多个场景。

- **Segue**: 如果应用中有多个场景，你使用 segues 从一个场景移动到另一个场景。JRNL 项目没有 segues，因为其 storyboard 文件中只有一个场景，但在本章后面的部分你会看到它们。

- **Auto Layout:** 作为开发者，你必须确保你的应用在不同屏幕尺寸的设备上看起来都很好。Auto Layout 帮助你根据指定的 **constraints** 布局你的 UI。例如，你可以设置一个约束以确保按钮无论屏幕尺寸如何都居中于屏幕，或使文本字段在设备从纵向旋转到横向时扩展到屏幕宽度。

现在你熟悉了 iOS 应用开发中使用的术语，让我们来了解一下你将构建的应用。

## A tour of the JRNL app

让我们快速了解一下你将构建的应用。*JRNL* 是一个期刊应用，允许用户撰写自己的个人日记，并可为每个日记条目存储照片或地图位置。用户还可以查看一张地图，显示靠近用户当前位置的条目位置。你将在下一节中看到应用中使用的所有屏幕及其整体流程。

## The Journal List screen

当应用启动时，你将看到 Journal List 屏幕：

![](img/0972ba626734a3986e095c788777818e_171_0.png)

Figure 10.6: Journal List screen

让我们研究一下此屏幕的不同部分。

屏幕底部的 UITabBar 实例（标签栏）显示 Journal 和 Map 按钮。Journal 按钮处于选中状态，你可以看到一个 table view 显示了 table view cells 中的日记条目列表。UISearchController 实例在屏幕顶部显示一个搜索栏。这允许你搜索特定的日记条目。

要添加新的日记条目，你点击屏幕顶部的 + 按钮。这将显示 Add New Journal Entry 屏幕。

## The Add New Journal Entry screen

当你在 Journal List 屏幕顶部点击 + 按钮时，你将看到 Add New Journal Entry 屏幕：

![](img/0972ba626734a3986e095c788777818e_172_0.png)

Figure 10.7: Add New Journal Entry screen

让我们研究一下此屏幕的不同部分。

