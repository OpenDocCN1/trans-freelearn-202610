## 索引

## Android: 面向开发者的编程

## Android: 面向开发者的编程

开发您自己的响应式、反应式且可立即部署的 Android 应用程序

三模块课程

![](img/35d01831c19688a44e50bce7c489007a_24_0.png)

伯明翰 - 孟买

## Android: 面向开发者的编程

版权所有 © 2016 Packt Publishing 保留所有权利。未经出版商事先书面许可，除非在关键文章或评论中引用简短段落，否则不得以任何形式复制、存储在检索系统中或以任何方式传播本课程的任何部分。

在本课程的准备工作中，已尽力确保所呈现信息的准确性。但是，本课程包含的信息按现状出售，不提供任何明示或暗示的担保。作者、Packt Publishing 及其经销商和分销商对因本课程直接或间接造成或声称造成的任何损害概不负责。

Packt Publishing 已努力通过适当使用大写字母提供本课程中提及的所有公司和产品的商标信息。但是，Packt Publishing 不能保证该信息的准确性。

出版日期：2016 年 8 月
出版者：Packt Publishing Ltd.
Liverpool Place
35 Liverpool Street Birmingham B3 2PB, UK.
ISBN 978-1-78712-369-4
www.packtpub.com

## 致谢

### 作者
- John Horton
- Helder Vasconcelos
- Raul Portales

### 审稿人
- Nayanesh Ramchandra Gupte
- Klaas Kabini
- Márton Kodok
- Paresh Mayani
- Gavin Matthews
- Sergio Viudes Carbonell
- Antonio Hernández Niñirola

### 内容开发编辑
Rohit Kumar Singh

### 图形
Abhinash Sahu

### 制作协调员
Aparna Bhagat

## 前言

您是否有一个很棒的应用程序创意，但不知道如何将其变为现实？您是否想要开始编程职业生涯，但尚未找到正确的入门方式？我们将帮助您掌握 Android 编程概念并开发自己的应用程序。无论您是 Android 初级开发者还是经验丰富的 Android 程序员，本课程都将探讨如何实现高效可靠的多线程 Android 应用程序。我们将研究 Android 开发者社区常用的最佳异步构造和技术，以在主线程之外执行计算密集型或阻塞任务，保持 UI 响应，告知用户进展情况，确保我们完成所开始的工作，利用强大的多核处理器，并且不浪费电池。您将学习使用太空射击游戏开发游戏的所有方面，该游戏将作为示例贯穿各章并随您一同发展。





## 此学习路径涵盖的内容

模块 1，面向初学者的 Android 编程，将向您介绍 Android 编程的基本概念。完成本模块后，您将准备好在 Android 和 Java 中开始构建自己的自定义应用程序。模块 2，异步 Android 编程，将帮助您学习如何管理多个线程之间的交互并避免并发和同步问题，并将帮助您构建行为良好的应用程序，拥有流畅、响应迅速的用户界面，以迅速的结果和始终新鲜的数据令用户欣喜。

模块 3，精通 Android 游戏开发，将指导您使用 Android SDK 从零构建一个实时游戏。从创建游戏引擎开始，到处理用户输入、进行高效绘图、实现碰撞检测、播放音效、使用动画，等等。

## 此学习路径所需内容

在任何主要操作系统上运行的 Android Studio 都可以使用本课程中的代码。Android Studio 是免费软件，Windows 的完整设置说明在开篇章节中。Android Studio 是推荐的开发工具，在出版时，最低系统要求如下：

## Windows

- Microsoft® Windows® 8/7/Vista/2003（32 位或 64 位）
- 最小 2 GB 内存；不过，推荐 4 GB 内存
- 400 MB 硬盘空间
- 至少 1 GB 用于 Android SDK、模拟器系统镜像和缓存
- 最小屏幕分辨率 1280 x 800
- Java 开发工具包 (JDK) 7
- 用于加速模拟器的可选项：支持 Intel® VT-x、Intel® EM64T (Intel® 64) 和 执行禁用 (XD) 位功能的 Intel® 处理器

## Mac OS X

- Mac® OS X® 10.8.5 或更高版本，最高至 10.9 (Mavericks)
- 最小 2 GB 内存；不过，推荐 4 GB 内存
- 400 MB 硬盘空间
- 至少 1 GB 用于 Android SDK、模拟器系统镜像和缓存
- 最小屏幕分辨率 1280 x 800
- Java 运行时环境 (JRE) 6
- Java 开发工具包 (JDK) 7
- 用于加速模拟器的可选项：支持 Intel® VT-x、Intel® EM64T (Intel® 64) 和 执行禁用 (XD) 位功能的 Intel® 处理器

在 Mac OS 上，使用 Java 运行时环境 (JRE) 6 运行 Android Studio 以获得优化的字体渲染。您随后可以将项目配置为使用 Java 开发工具包 (JDK) 6 或 JDK 7。

## Linux

- GNOME 或 KDE 桌面环境
- GNU C Library (glibc) 2.15 或更高版本
- 最小 2 GB 内存；不过，推荐 4 GB 内存
- 400 MB 硬盘空间
- 至少 1 GB 用于 Android SDK、模拟器系统镜像和缓存
- 最小屏幕分辨率 1280 x 800
- Oracle® Java 开发工具包 (JDK) 7

在 Ubuntu® 14.04, Trusty Tahr（能够运行 32 位应用程序的 64 位发行版）上进行了测试。

对于模块 2，若要跟随示例并进行实验，您需要一台安装了 Java 7（或 8）SE 开发工具包和 Android 软件开发工具包版本 9 或更高版本（要尝试所有示例，至少需要版本 21）的开发计算机。

您还需要 Android Studio IDE。示例是使用 Google 的全新 Android Studio IDE 开发的，并使用其集成构建系统 Gradle。虽然您可以使用 Android SDK 提供的模拟器运行示例，但它远不能替代真实设备。一部实体 Android 设备是开发和测试 Android 应用程序更快速、更愉快的方式！许多示例可在运行 2.3（姜饼，GingerBread）以来任何版本的 Android 设备上运行。一些示例演示了较新的 API，因此需要较新的 Android 版本——最高可至 Android 5（棒棒糖，Lollipop）。

专门针对模块 3，您需要最新版本的 Android Studio 以及适用于棒棒糖（Lollipop）或更新版本（API 级别 22）的 Android SDK，可通过 Android Studio 下载。

## 此学习路径适合人群

如果您是 iOS 开发者或任何其他开发者/程序员，并希望尝试在 Android 平台上开发应用程序，那么本课程适合您。无需任何先前的编程经验，因为本课程将从头开始，一路指导您掌握 Android 编程的高级概念。

## 读者反馈

我们始终欢迎来自读者的反馈。请告诉我们您对本课程的看法——您喜欢或不喜欢什么。读者反馈对我们非常重要，因为这有助于我们开发出您能真正从中获益的书籍。

要向我们发送一般性反馈，只需发送电子邮件至 <feedback@packtpub.com>，并在邮件主题中注明课程名称。

如果您在某个主题方面有专长，并有兴趣编写或参与编写书籍，请参阅我们的作者指南 [作者指南](http://www.packtpub.com/authors)。

## 客户支持

既然您已成为 Packt 课程的自豪拥有者，我们有许多资源可以帮助您充分利用您的购买。

## 下载示例代码

您可以从您在 http://www.packtpub.com 上的账户中下载本课程的示例代码文件。如果您在其他地方购买了本课程，可以访问 http://www.packtpub.com/support 并注册，以便将文件直接发送到您的邮箱。

您可以按照以下步骤下载代码文件：

1. 使用您的电子邮件地址和密码登录或注册我们的网站。
2. 将鼠标指针悬停在顶部的 **支持** 选项卡上。
3. 单击 **代码下载与勘误**。
4. 在 **搜索** 框中输入课程名称。
5. 选择您要下载代码文件的课程。
6. 从下拉菜单中选择您购买本课程的渠道。
7. 单击 **代码下载**。

您也可以通过点击 Packt Publishing 网站上课程页面的 **代码文件** 按钮来下载代码文件。该页面可通过在 **搜索** 框中输入课程名称来访问。请注意，您需要登录到您的 Packt 账户。

文件下载完成后，请确保使用以下最新版本解压或提取文件夹：

- 适用于 Windows 的 WinRAR / 7-Zip
- 适用于 Mac 的 Zipeg / iZip / UnRarX
- 适用于 Linux 的 7-Zip / PeaZip

本课程的代码包也托管在 GitHub 上，地址为 https://github.com/PacktPublishing/Android-Programming-for-Developers。我们丰富的书籍、视频和课程目录中还有其他代码包，可在 https://github.com/PacktPublishing/ 查看。欢迎查看！

## 勘误

尽管我们已竭力确保内容的准确性，但错误难免会发生。如果您在我们的某个课程中发现错误——也许是文本或代码中的错误——我们将不胜感激，如果您能将此情况报告给我们。这样做，您可以帮助其他读者避免困扰，并帮助我们改进本课程的后续版本。如果您发现任何勘误，请访问 [提交勘误](http://www.packtpub.com/submit-errata)，选择您的课程，单击 **勘误提交表单** 链接，并输入您的勘误详情。一旦您的勘误得到验证，您的提交将被接受，勘误将上传到我们的网站，或添加到该书名下勘误部分的现有勘误列表中。

要查看先前提交的勘误，请访问 [勘误支持页面](https://www.packtpub.com/books/content/support)，并在搜索字段中输入课程名称。所需信息将出现在 **勘误** 部分下。

## 盗版

互联网上受版权保护材料的盗版是一个跨所有媒体的持续问题。在 Packt，我们非常重视对我们的版权和许可的保护。如果您在互联网上的任何形式中发现我们作品的非法副本，请立即向我们提供位置地址或网站名称，以便我们可以采取补救措施。

请通过 <copyright@packtpub.com> 联系我们，并提供涉嫌盗版材料的链接。

我们感谢您在保护我们的作者以及我们为您提供有价值内容的能力方面提供的帮助。





## Questions

如果您对本课程的任何方面有问题，可以联系 <questions@packtpub.com>，我们将竭力解决问题。
