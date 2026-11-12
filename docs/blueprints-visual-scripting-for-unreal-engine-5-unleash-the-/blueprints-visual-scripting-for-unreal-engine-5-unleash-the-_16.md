## 第 12 章：构建与发布

作为游戏开发者成长的最佳方式之一，就是与他人分享你的作品，以便获得关于如何改进设计和内容的反馈。早期的优先事项之一应该是创建可共享的游戏构建版本，以便其他人可以自行游玩。幸运的是，虚幻引擎 5 使得为多个平台创建游戏构建版本变得极其简单。在本章中，我们将学习如何优化游戏设置以及针对目标桌面平台进行构建的过程。在此过程中，我们将涵盖以下主题：

- 优化图形设置
- 设置游戏以供他人游玩
- 将游戏打包为构建版本

到本章结束时，你将拥有一个打包好的游戏版本，可以分享并在其他机器上安装。

### 优化图形设置

在创建构建版本（即针对特定平台进行了优化的游戏版本）之前，你应该更改游戏的图形设置，以确保它们适合你的目标机器。虚幻引擎 5 中的图形设置标识为 **引擎可缩放设置**。此设置界面由多个图形设置组成，每个设置决定游戏中某个元素的最终视觉质量。对于任何游戏，高质量效果和视觉效果与游戏性能（以帧率衡量）之间存在权衡。

帧率较低的游戏从游戏性角度来看感觉很糟糕，即使机制本身很稳固。因此，重要的是要平衡让游戏尽可能美观的愿望，与理解游戏对玩家运行游戏的机器会产生什么性能影响的需求之间的关系。

由于 PC 和 macOS 计算机的硬件性能各不相同，许多针对这些平台的游戏使用自定义菜单设置，允许玩家自行调整游戏的图形设置。然而，我们创建的游戏仅使用非常简单的资产和相对受限的关卡大小，因此我们只需在生成分发构建版本之前定义一些可行的默认值即可。

按照以下步骤修改 **引擎可缩放设置**：

- 单击位于关卡编辑器工具栏右侧的 **设置** 按钮。

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_423_0.png)

图 12.1 – 访问引擎可缩放设置

- 将鼠标悬停在 **引擎可缩放设置** 上，以查看 **质量** 设置的弹出显示，如下面的屏幕截图所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_424_0.png)

图 12.2 – 引擎可缩放设置元素

- 此菜单顶部的按钮，从 低 到 史诗级，充当设置的预设，基于你在运行时希望针对的性能与质量的广泛级别。单击 低 按钮会将所有质量设置设置为最小，以换取最不具视觉吸引力的设置，来为你提供尽可能好的性能。史诗级是谱系的另一端，将所有引擎质量设置提升到最大，具体性能损耗取决于你选择使用的资产。

- 电影级按钮会将所有质量设置设置为电影级质量，用于渲染过场动画。此设置不打算在游戏过程中或运行时使用。

- 自动按钮会检测你当前运行编辑器的机器的硬件，并将图形设置调整到在性能与适合你机器的图形质量之间取得良好平衡的水平。如果你打算针对与开发机器性能大致相当的硬件，使用自动设置可以作为为构建版本建立图形设置的一种简单方法。

- 如果你希望单独调整设置，可以使用以下对其功能的简要描述来获得帮助：

- **分辨率缩放**：此设置会导致引擎以低于玩家目标分辨率的分辨率渲染游戏，并使用软件将游戏放大到目标分辨率。这可以提高游戏性能，但代价是在较低分辨率缩放时会出现感知模糊。

- **视距**：这决定了从摄像机位置开始对象将被渲染的距离。此距离之外的对象将不会被渲染。较短的视距可以提高性能，但可能导致对象在越过视距边界时突然弹入视野。

- **抗锯齿**：此设置可软化世界中 3D 对象的锯齿状边缘，这可以显著改善游戏的外观。在较高设置下，你会看到更少的锯齿边缘，但这会降低性能。

- **后期处理**：此设置更改在场景创建后应用到屏幕上的多个滤镜的基线质量设置，例如运动模糊和光晕效果。

- **阴影**：这更改了多个捆绑设置的基础质量，这些设置共同决定游戏中阴影的外观。高度详细的阴影通常会对性能产生显著影响。

- **纹理**：此设置将影响引擎管理游戏中使用的纹理的过程。如果游戏中有许多大纹理，降低此设置可以帮助避免显存耗尽，从而提高性能。

- **效果**：此设置更改应用到游戏中的多个特效（例如材质反射和半透明效果）的基线质量设置。

- **植被**：此设置将影响游戏中使用的植被质量。

- **着色**：此设置将影响材质的质量。

最终，优化游戏性能的最佳方法是定期在打算让人们游玩的机器上测试游戏。如果你注意到性能迟缓，请记录发生位置。如果游戏性能始终较低，你可能需要减少一些后期处理或抗锯齿效果。如果性能仅在关卡的某些区域较低，你可能需要查看降低该区域的对象密度或降低特定游戏模型的质量。

既然我们知道如何调整图形设置，那么在构建之前先自定义一些项目设置。

### 设置游戏以供他人游玩

虚幻引擎 5 提供了广泛的平台供你选择来构建游戏，并且随着引擎新版本的发布和新技术的出现，此列表将继续扩展。你可以在 Windows、macOS、iOS、Android 和 Linux 上部署游戏。该引擎支持为各种虚拟现实平台创建内容，例如 Oculus Quest。还支持第 8 代和第 9 代游戏主机，但你需要成为拥有适当开发套件的注册主机开发者。

每个平台都有与其相关的独特要求和游戏开发最佳实践。移动游戏具有更高的优化要求，以使游戏运行良好。

我们可以自定义一些设置，这些设置将决定我们的项目在目标机器上的显示方式。

按照以下步骤自定义项目设置：

- 单击关卡编辑器工具栏中的 **设置** 按钮，然后单击 **项目设置...**，如下所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_427_0.png)

图 12.3 – 访问项目设置

- 在 **项目设置** 内，你将在左侧面板中看到广泛的选项，用于自定义游戏、引擎和平台交互的不同方面。默认情况下，将打开 **项目 - 描述** 页面。在这里，你可以自定义项目名称、将在虚幻引擎项目选择器中显示的缩略图，以及项目及其创建者或发布者的简要描述，如此屏幕截图所示：



![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_428_0.png)

### 图 12.4 – 项目 - 描述页面

- 3. 点击左侧面板中的 Maps & Modes 将带你进入一个页面，你可以在该页面上确定游戏默认加载哪个地图。我们的游戏只有一张地图，因此这个选择很简单，但你通常需要指定一张专门用于主菜单屏幕的地图作为第一个加载的地图。当你创建包含多个地图的游戏时，需要确保第一个加载的地图能够管理游戏体验中接下来加载哪个地图。

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_429_0.png)

### 图 12.5 – 项目 - 地图与模式页面

- 4. 最后，点击此构建针对的平台将带你进入该平台的自定义页面。

- 5. 在以下截图所示的 Windows 示例中，可更改启动画面和游戏的图标图像。移动和主机平台目标将有更多可更改的选项，这些选项将针对每个平台特定。

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_430_0.png)

### 图 12.6 – 平台 - Windows 页面

- 6. 使用你想要为游戏使用的图像替换默认的 **SPLASH** 和 **ICON** 设置。这可以简单到只是游戏中编辑过的截图，或者你可以展示专门为图标和启动画面制作的自定义艺术作品。

Once you are satisfied with your project settings, close the **Project Settings** window so we can start packaging our project.

一旦你对项目设置感到满意，关闭 **项目设置** 窗口，以便我们可以开始打包项目。

### Packaging the game into a build

### 将游戏打包为构建版本

Creating a distributable form of your game for one of these platforms involves a process called **packaging**. Packaging takes all the code and assets of the game and sets them up in the proper format to be able to perform on the selected platform. We will be following the path to making a Windows or macOS release of your game.

为这些平台之一创建游戏的可分发形式涉及一个称为 **打包** 的过程。打包将获取游戏的所有代码和资源，并将其设置为适当的格式，以便在所选平台上运行。我们将遵循制作游戏的 Windows 或 macOS 发行版的路径。

Note

注意

It is important to note that Unreal Engine 5 can only create Windows builds from a copy of the engine running on a Windows system, and OS X builds from copies installed on a macOS running OS X. Thus, the platforms that you can target with your game will be partially limited by the machine you are developing the game on. If you are developing on a Windows PC and wish to create an OS X build of the game, you can install Unreal Engine 5 on a macOS and copy your project files to this new machine. From there, you will be able to generate an OS X build, with no further changes required.

需要注意的是，Unreal Engine 5 只能在运行 Windows 系统的引擎副本上创建 Windows 构建版本，以及在运行 OS X 的 macOS 上安装的副本中创建 OS X 构建版本。因此，你可以用游戏来定位的平台将部分受限于你开发游戏的机器。如果你在 Windows PC 上开发并希望创建游戏的 OS X 构建版本，可以在 macOS 上安装 Unreal Engine 5，并将项目文件复制到这台新机器。之后，你将能够生成 OS X 构建版本，无需进一步更改。

These are the steps to package your game to be played on a particular platform:

以下是将游戏打包以在特定平台上运行的步骤：

- 1. Click the **Platforms** button in the toolbar, hover over the desired platform, and then click on **Package Project**, as follows:

- 1. 单击工具栏中的 **平台** 按钮，将鼠标悬停在所需平台上，然后单击 **打包项目**，如下所示：

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_432_0.png)

### Figure 12.7 – Packaging the project for Windows

### 图 12.7 – 为 Windows 打包项目

- 2. Once you click on a platform, you will be prompted to select a location on your computer to store the build you make. After selecting a location, you will see a popup that tells you that the engine is packaging the project. If something goes wrong in the packaging process, you will be shown the details of the error in the output log window that pops up. Packaging a project can take a bit of time, depending on how complex and large the project is, but if you don't encounter any errors, then you will eventually see a message saying that the packaging is complete. Congratulations – you have created a packaged copy of your game!

- 2. 单击平台后，系统将提示你在计算机上选择一个位置来存储你制作的构建版本。选择位置后，你将看到一个弹出窗口，告知引擎正在打包项目。如果打包过程中出现问题，将在弹出的输出日志窗口中显示错误详细信息。打包项目可能需要一些时间，具体取决于项目的复杂性和大小，但如果你没有遇到任何错误，最终将看到一条消息，说明打包已完成。恭喜 – 你已创建游戏的打包副本！

- 3. Navigate to the folder where you chose to store your build. On macOS, open the folder called MacNoEditor and double-click on the application to launch it. For Windows, open the WindowsNoEditor folder and double-click on the executable to run the game. Take a moment to go through the game you created in its final form and reflect on just how far you have come. You now have a functional game that other people can play and enjoy. Making even simple games is no easy feat, so you should feel proud of your accomplishment!

- 3. 导航到你选择存储构建版本的文件夹。在 macOS 上，打开名为 MacNoEditor 的文件夹并双击应用程序以启动它。在 Windows 上，打开 WindowsNoEditor 文件夹并双击可执行文件以运行游戏。花点时间体验一下你创建的游戏的最终形式，回顾一下你已经走了多远。你现在拥有一个其他人可以玩和享受的功能性游戏。即使制作简单的游戏也不容易，所以你应该为自己的成就感到自豪！

We packaged the project using the default settings, but there are a lot of options that you can modify in the packaging.

我们使用默认设置打包了项目，但在打包中有许多选项可以修改。

### Build configurations and packaging settings

### 构建配置和打包设置

In this section, we will see some of the settings available for build and packaging.

在本节中，我们将了解构建和打包中可用的一些设置。

The Platforms submenu has the Packaging Settings option, which displays the Project - Packaging page with configuration options that can be modified.

平台子菜单中有打包设置选项，该选项将显示项目 - 打包页面，其中包含可修改的配置选项。

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_434_0.png)

### Figure 12.8 – Project - Packaging page

### 图 12.8 – 项目 - 打包页面

The **PACKAGING** category contains a lot of technical options that can be used to optimize the packaging. The **Build Configuration** option defines how the build will be done. In Blueprint-only projects, you have two options: **Development** and **Shipping**. Development builds contain information that is used in debugging to help you find errors. Shipping builds are cleaner as they do not have debug information and should be used to create the final version of the game that will be distributed.

**打包** 类别包含许多可用于优化打包的技术选项。**构建配置** 选项定义构建的执行方式。在仅使用蓝图的项目中，你有两个选项：**开发** 和 **发布**。开发构建包含用于调试的信息，以帮助你查找错误。发布构建更干净，因为它们不包含调试信息，并应用于创建将要分发的游戏最终版本。

The **Staging Directory** property is used to define the folder where the packaged build will be stored. The **For Distribution** option is required when submitting a game to the App Store or Google Play Store.

**暂存目录** 属性用于定义存储打包构建版本的文件夹。当将游戏提交到 App Store 或 Google Play Store 时，需要 **用于分发** 选项。

You can now close the **Project - Packaging** page.

现在可以关闭 **项目 - 打包** 页面了。

When you want to move the project to another computer or to pass the project to someone else, you can use the **Zip Project** option available in the File menu of the Level Editor. The Zip Project option copies and compresses the essential project files.

当你想要将项目移动到另一台计算机或将项目传递给他人时，可以使用关卡编辑器文件菜单中提供的 **压缩项目** 选项。**压缩项目** 选项会复制并压缩基本项目文件。

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_435_0.png)

### Figure 12.9 – Zip Project

### 图 12.9 – 压缩项目

There are many packaging options and some of them are platform-specific. Take the time to study the various packaging options for your game's target platform.

有许多打包选项，其中一些是平台特定的。花时间研究游戏目标平台的各种打包选项。

### Summary

### 总结

In this chapter, we discussed creating playable builds of the game we created across multiple platforms. We learned how to optimize the graphics settings and discussed how to set up the game to be played by others. Finally, we saw how to change the build configurations and packaging settings.

在本章中，我们讨论了如何在多个平台上创建我们制作的游戏的可玩构建版本。我们学习了如何优化图形设置，并讨论了如何设置游戏以供他人游玩。最后，我们了解了如何更改构建配置和打包设置。

With what you've learned in this chapter, you will be able to better present your project and distribute it on different platforms so that others can enjoy it.

通过本章所学的内容，你将能够更好地展示你的项目，并在不同平台上分发它，以便其他人可以享受它。

This chapter concludes Part 3 and ends our implementation of the playable shooter game. Part 4 will cover advanced Blueprints concepts. In the next chapter, we will learn about data structures and flow control.

本章结束第 3 部分，并结束了我们对可玩射击游戏的实现。第 4 部分将介绍高级蓝图概念。在下一章中，我们将学习数据结构和流程控制。

### Quiz

### 测验

- 1. You can adjust the graphics settings of your game in the **Engine Scalability Settings** panel.

- 1. 你可以在 **引擎缩放设置** 面板中调整游戏的图形设置。

- a. True

- a. 正确

- b. False

- b. 错误

- 2. In Unreal Engine 5, you can package your game for consoles without being a registered console developer.

- 2. 在 Unreal Engine 5 中，你无需成为注册主机开发者即可为主机打包游戏。

- a. True

- a. 正确

- b. False

- b. 错误

- 3. To package your game, you need to access a menu by clicking the **Platforms** button.

- 3. 要打包游戏，你需要通过单击 **平台** 按钮来访问菜单。

- a. True

- a. 正确

- b. False

- b. 错误

- 4. Which option is not a **Build Configuration** option in Blueprint-only projects?

- 4. 以下哪个选项不是仅使用蓝图项目中的 **构建配置** 选项？

- a. Development

- a. 开发

- b. Shipping

- b. 发布

- c. Test

- c. 测试

- 5. The **Staging Directory** property is used to define the folder where the packaged build will be stored.

- 5. **暂存目录** 属性用于定义存储打包构建版本的文件夹。

- a. True

- a. 正确

- b. False

- b. 错误

# Part 4: Advanced Blueprints
