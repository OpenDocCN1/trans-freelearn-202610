

6. 从 **Device** 菜单中选择 **Rotate Left**，你会看到两列：

![](img/0972ba626734a3986e095c788777818e_505_0.png)

*Figure 22.39: Simulator showing iPhone screen rotated with two columns*

7. 停止你的应用并在 **Destination** 菜单中选择 **My Mac (Designed for iPad)**。运行你的应用，它应该显示两列：

![](img/0972ba626734a3986e095c788777818e_505_1.png)

*Figure 22.40: Mac app with two columns*

> 你需要一个免费或付费的 Apple 开发者账号才能在你的 Mac 上运行你的应用。

你已修改你的应用以使用集合视图替代表格视图，并且已使其能够在不同设备上运行时动态修改集合视图单元格大小。干得好！

## 23 Adopt Liquid Glass

在 Apple 2025 年的 **Worldwide Developers Conference (WWDC)** 上，他们在所有平台上引入了新的设计。其关键元素是一种名为 **Liquid Glass** 的新材质。UIKit 的外观和感觉已随此设计更新，因此你只需使用最新 SDK 重新编译应用即可。然而，仍有一些事情你可以做来完善应用的用户体验。

在本章中，你将更新 *JRNL* 应用以利用新设计。首先，你将了解利用 Liquid Glass 的新设计系统。接下来，你将修改你的应用以采用新设计。

在本章结束时，你将学会如何为自己的应用实现新的设计系统。

本章将涵盖以下主题：

- Meet Liquid Glass
- Modify your app

### Technical requirements

你将继续在 *Chapter 22, Collection Views* 中修改的 JRNL 项目上工作。

本章的资源文件和完成的 Xcode 项目位于本书代码包的 Chapter 23 文件夹中，可在此下载：

```
https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E
```

观看以下视频以查看代码运行情况：

https://youtu.be/jkd0y2NbBbM

让我们从下一节开始了解更多关于新设计系统和 Liquid Glass 的信息。

### Meet Liquid Glass

Apple 的新 Liquid Glass 材质结合了玻璃的光学特性与流动感。控件和导航元素放置在不同的功能层上，并且用户体验被修改以使内容成为焦点。

要了解新设计系统的概览，请观看此视频：https://developer.apple.com/videos/play/wwdc2025/219/

要了解新设计系统的最佳实践，请观看此视频：https://developer.apple.com/videos/play/wwdc2025/356/

使用最新 SDK 重新编译的应用用户界面会自动更新。然而，你可能需要对应用进行更改以充分拥抱新设计。

要了解如何将应用更新为新设计，请观看此视频：https://developer.apple.com/videos/play/wwdc2025/284/

在本书中，你一直在使用标准用户界面元素来构建你的应用，除了你在第 19 章 Custom Views 中构建的 RatingView 元素。因此，当你使用 Xcode 26 构建和运行 JRNL 应用时，它会自动采用融入 Liquid Glass 的新设计。话虽如此，你仍可以对应用进行一些调整，你将在下一节学习如何做到这一点。

### Modify your app

当你使用 Xcode 26 构建和运行 JRNL 应用时，你可以看到它已自动采用 Apple 的新设计，融入了 Liquid Glass。仍有一些修改可以进行以充分拥抱新的外观和感觉，具体如下：

- Automatically minimize the tab bar on scroll down
- Add a subtitle showing the number of journal entries on the Journal List screen
- Add a Liquid Glass effect to the star buttons in RatingView

让我们看看如何实现这些更改，从下一节开始介绍向下滚动时最小化标签栏。

### Minimize the tab bar on scroll down

在 iOS 26 中，标签栏已使用 Liquid Glass 外观更新。它浮动在你的内容之上，并且可以配置为在向上或向下滚动时最小化，保持对内容的关注。要在向下滚动时最小化标签栏，请按照以下步骤操作：

1. 在 Project navigator 中，单击 **JournalListViewController** 文件（位于 **Journal List Scene** 组中）。在 viewDidLoad() 方法中添加以下行：

```swift
// MARK: - View controller lifecycle
override func viewDidLoad() {
    super.viewDidLoad()
    tabBarController?.tabBarMinimizeBehavior = .onScrollDown
    setupCollectionView()
}
```

2. 构建并运行你的项目。你会看到向下滚动时标签栏会最小化：

![](img/0972ba626734a3986e095c788777818e_511_0.png)

*Figure 23.1: Minimized tab bar on the Journal List screen*

> 此功能在 iOS 26 的 beta 版本中可用，但在正式版中似乎无法工作。Apple 可能会在以后恢复此功能。

太好了！在下一节中，你将学习如何在 Journal List 屏幕中添加显示日记条目数量的副标题。

### Add a subtitle to the Journal List screen

在 iOS 26 中，每个视图控制器的 UINavigationItem 对象获得了一个新的 subtitle 属性，它显示在标题下方。你将添加代码以在 Journal List 屏幕的标题下方显示日记条目数量。请按照以下步骤操作：

1. 在 Project navigator 中单击 **JournalListViewController** 文件。在 setupCollectionView() 方法之后添加一个新方法，以在 Journal List 屏幕的副标题中显示日记条目数量：

```swift
func updateSubtitleCount() {
    let noOfJournalEntries = SharedData.shared.numberOfJournalEntries
    switch noOfJournalEntries {
    case 0:
        navigationItem.subtitle = "No journal entries"
    case 1:
        navigationItem.subtitle = "1 journal entry"
    default:
        navigationItem.subtitle = "\(noOfJournalEntries) journal entries"
    }
}
```

2. 在 viewDidLoad() 中在闭合大括号之前调用 updateSubtitleCount()。这将在应用启动时在 Journal List 屏幕中显示日记条目数量：

```swift
navigationItem.searchController = search
updateSubtitleCount()
}
```

3. 当日记条目被删除时，需要更新显示的日记条目数量。修改 collectionView(_:contextMenuConfigurationForItemAt:point:) 以在日记条目被删除时调用 updateSubtitleCount()：

```swift
collectionView.reloadData()
self.updateSubtitleCount()
}
return UIMenu(children: [delete])
```

## Technical requirements

你将继续在 *Chapter 23, Adopt Liquid Glass* 中修改的 JRNL 项目上工作。

本章的资源文件和完成的 Xcode 项目位于本书代码包的 Chapter 24 文件夹中，可在此下载：

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

观看以下视频以查看代码运行情况：

https://youtu.be/sShKNjo2jRs

让我们从下一节开始了解 Icon Composer。

## About Icon Composer

在 WWDC 2025 期间引入，Icon Composer 用于创建应用图标的单一文件表示。此文件包含所有 Apple 平台的所有图标和 widget 样式外观和尺寸。要创建初始图标 artwork，你可以使用你喜欢的设计工具，如 Photoshop 或 Canva。然后你将导出 artwork 的一个或多个图层并将其导入 Icon Composer。在 Icon Composer 中，你可以组织和美化 artwork，充分利用 Liquid Glass 的动态属性。最后，你将把图标添加到你的项目中。

要了解有关如何使用 Icon Composer 的更多信息，请观看此视频：
https://developer.apple.com/videos/play/wwdc2025/361/

Icon Composer 文档可在以下链接获取：
https://developer.apple.com/documentation/Xcode/creating-your-app-icon-using-icon-composer

![](img/0972ba626734a3986e095c788777818e_518_0.png)

让我们在下一节中了解如何为图标的每个图层生成单独的图像。

## Creating images for each icon layer

创建图标的典型方法将使用 Adobe Photoshop 或 Canva 等应用。话虽如此，任何可以生成图标的单独图层作为单独文件的应用都可以使用。你将使用 Keynote，为 Apple Mac 免费，打开包含示例图标设计的演示文稿并将其导出为可在 Icon Composer 中使用的单独图层。

