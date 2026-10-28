
请按照以下步骤操作：

- 1. 如果你尚未安装 Keynote 应用，请通过此链接下载并安装：https://apps.apple.com/us/app/keynote/id409183694?mt=12

- 2. 在此链接下载本章的项目文件，这些文件位于本书代码包的 Chapter24 文件夹中：https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

- 3. 打开 Chapter24 文件夹，打开 resources 文件夹，然后打开 JRNLImages 文件。你将看到一个包含四张幻灯片的 Keynote 演示文稿，如下图所示：

   ![](img/0972ba626734a3986e095c788777818e_519_0.png)

   图 24.1：JRNLImages 文件的内容

- 4. 如果第一张幻灯片未被选中，请将其选中。点击工具栏上的“格式”按钮。注意“背景”设置为“无填充”，本演示文稿中的所有幻灯片都是如此。这是确保导出时背景透明所必需的：

   ![](img/0972ba626734a3986e095c788777818e_520_0.png)

   图 24.2：背景设置为“无填充”的幻灯片

- 5. 点击工具栏上的“文稿”按钮。注意幻灯片大小设置为 1024 x 1024，这是 iOS 26 中图标所需的尺寸：

   ![](img/0972ba626734a3986e095c788777818e_520_1.png)

   图 24.3：Keynote 文稿幻灯片大小设置为 1024 x 1024

- 6. 从“文件”菜单中选择“导出为” | “图像...”：

   ![](img/0972ba626734a3986e095c788777818e_521_0.png)

   图 24.4：Keynote 文件菜单中选择了“导出为” | “图像...”

## 使用 Icon Composer 创建图标

此时，你的图标各图层已保存为带有透明背景的 PNG 文件。现在你将使用 Icon Composer 来组合和修改这些文件，以创建你的应用图标。请按照以下步骤操作：

- 1. 从 Xcode 菜单中，选择“打开开发者工具” | “Icon Composer”：

   ![](img/0972ba626734a3986e095c788777818e_523_0.png)

   图 24.6：Xcode 菜单中选择了“打开开发者工具” | “Icon Composer”

- 2. 如果看到显示 **Icon Composer 许可协议** 的屏幕，请点击 **同意**：

   ![](img/0972ba626734a3986e095c788777818e_524_0.png)

   *图 24.7：Icon Composer 许可协议屏幕，其中“同意”按钮高亮显示*

- 3. Icon Composer 将启动并显示文件选择对话框。点击 **新建文稿**。

- 4. 出现一个新的 Icon Composer 文稿。打开包含已导出 PNG 文件的文件夹，并将它们拖到 Icon Composer 文稿的左侧：

   ![](img/0972ba626734a3986e095c788777818e_525_0.png)

   图 24.8：Icon Composer 文稿，显示 PNG 文件的拖放目标位置

- 5. 确认 Icon Composer 已自动组合你的应用图标，并对每个图像应用了 Liquid Glass 效果：

   ![](img/0972ba626734a3986e095c788777818e_525_1.png)

   图 24.9：Icon Composer 文稿，显示应用图标

## 将图标添加到你的项目中

至此，你已经为项目创建了图标。接下来，你将使用 Xcode 导入图标，并使用 Simulator 预览它在各种模式下的显示效果。请按照以下步骤操作：

- 1. 打开你在上一章（第 23 章“采用 Liquid Glass”）中修改的 JRNL 项目。

- 2. 将 Icon Composer 文件拖到项目的项目导航器中。

   ![](img/0972ba626734a3986e095c788777818e_528_0.png)

   图 24.14：Xcode 项目，显示 Icon Composer 文件的拖放目标位置

- 3. 在“选择这些文件的添加选项”窗口中，按截图所示进行配置，然后点击“完成”：

   ![](img/0972ba626734a3986e095c788777818e_528_1.png)

   图 24.15：“选择这些文件的添加选项”窗口

- 4. 在项目导航器中，点击 JRNL 项目并选择项目目标。在“通用”标签页中，将“应用图标”设置为上一步中添加的 Icon Composer 文件的名称：

   ![](img/0972ba626734a3986e095c788777818e_529_0.png)

   图 24.16：Xcode 项目，显示“应用图标”已设置为 Icon Composer 文件的名称

- 5. 构建并运行你的项目，然后停止运行。你将在 Simulator 屏幕上看到你的应用的新图标：

   ![](img/0972ba626734a3986e095c788777818e_529_1.png)

   图 24.17：Simulator 显示 JRNL 应用的新图标

- 6. 从 Simulator 菜单栏的 **“功能”** 菜单中选择 **“切换外观”**。Simulator 将切换到深色模式，**JRNL** 图标也会自动变为深色变体：

   ![](img/0972ba626734a3986e095c788777818e_530_0.png)

   图 24.18：处于深色模式的 Simulator，显示更改后的 JRNL 应用图标

你已使用 Icon Composer 创建了一个图标，并成功将其添加到 JRNL 项目中！太棒了！

## 小结

在本章中，你学习了如何使用 Icon Composer 为 JRNL 应用构建图标。

你首先使用 Keynote 为图标的每个图层生成带有透明背景的 PNG 文件。接着，你将 PNG 文件添加到 Icon Composer，并调整了每个图层的属性。最后，你将它添加到项目中，并在 Simulator 中测试了其外观。

现在你知道了如何使用 Icon Composer 为自己的应用创建图标。

在本节对 SwiftUI 的简要介绍中，你已经了解了如何使用 SwiftUI 构建 JRNL 应用的简化版本。

在下一章中，你将了解 **Apple Intelligence**，以及如何使用它来生成代码并为你的应用添加一些很酷的功能。

## 加入我们的 Discord

与其他读者、专家和本书作者一同阅读这本书。

提出问题、为其他读者提供解决方案、通过“Ask Me Anything”问答环节与作者交流，以及更多精彩内容。扫描二维码或访问链接加入社区。

https://packt.link/LdIDT/

![](img/0972ba626734a3986e095c788777818e_531_0.png)

## 25 Apple Intelligence

在 2024 年苹果全球开发者大会（WWDC）上，苹果推出了 **Apple Intelligence**——一个将强大生成模型置于苹果设备中的个人智能系统，为你的应用带来全新 AI 驱动的功能。这些功能包括 **写作工具（Writing Tools）**、**图像游乐场（Image Playground）**、**Genmoji** 和 **支持 App Intents 的 Siri**。

在本章中，你将在应用中实现 Apple Intelligence 功能。首先，你将了解 Apple Intelligence 及其功能。接下来，你将看到预测性代码补全如何帮助你编写应用。之后，你将了解写作工具（Writing Tools）并查看它在应用中的运作方式。然后，你将在应用中实现图像游乐场（Image Playground）。最后，你将了解 Genmoji，并查看它在应用中的运作方式。

学完本章后，你将学会如何在 Xcode 和应用中使用 Apple Intelligence 功能。

本章将涵盖以下主题：

- 关于 Apple Intelligence
- 在 Xcode 中使用预测性代码补全
- 在你的应用中实现写作工具（Writing Tools）
- 在你的应用中实现图像游乐场（Image Playground）
- 在你的应用中实现 Genmoji

### 技术要求

你将继续处理你在 *第 24 章“Icon Composer”* 中修改过的 JRNL 项目。

本章的资源文件和完整 Xcode 项目位于本书代码包的 Chapter 25 文件夹中，可在此处下载：
https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

观看以下视频，了解代码的实际运行效果：
https://youtu.be/wgfrKZaj4VI

让我们从下一节开始了解 Apple Intelligence。

### 关于 Apple Intelligence

Apple Intelligence 是一个人工智能平台，由设备端和服务器端处理组成，将支持一系列出色的新功能，帮助用户沟通、工作和表达自我。这些功能包括以下内容：

- 预测性代码补全，帮助你使用 Xcode 编写代码
- 写作工具（Writing Tools），帮助用户校对、改写和总结文本
- 图像游乐场（Image Playground），允许用户创建有趣好玩的图像
- Genmoji，让用户能够创建适合任何场合的表情符号
- 支持 App Intents 的 Siri，让开发者能够赋予 Siri 在其应用内执行操作的能力

Apple Intelligence 对所有 Apple 用户免费，并且苹果在 2025 年持续为其添加新功能。

要了解 Apple Intelligence 的最新更新，请观看此视频：
https://developer.apple.com/videos/play/wwdc2025/360/

你可以通过此链接查看 Apple Developer 上关于 Apple Intelligence 的文档：
https://developer.apple.com/apple-intelligence/

在下一节中，你将了解 Apple Intelligence 如何帮助你为应用编写代码。
