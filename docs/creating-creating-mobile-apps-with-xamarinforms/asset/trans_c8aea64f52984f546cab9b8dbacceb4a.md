

## 共享代码

使用单一编程语言面向多个平台的优势，在于能够在应用程序之间共享代码。

在共享代码之前，必须为此目的进行结构化设计。尤其是自从图形用户界面广泛使用以来，程序员们已经意识到将应用程序代码划分为功能层的重要性。也许最有用的划分是用户界面代码与底层数据模型和算法之间的划分。流行的 MVC（Model-View-Controller）应用程序架构将这种代码分离正式化为 Model（底层数据）、View（数据的可视化表示）和 Controller（处理用户输入）。

MVC 起源于 20 世纪 80 年代。后来，MVVM（Model-View-ViewModel）架构基于现代 GUI 有效地对 MVC 进行了现代化改造。MVVM 将代码分为 Model（底层数据）、View（用户界面，包括视觉效果和输入）和 ViewModel（管理 Model 与 View 之间传递的数据）。

在开发面向多个移动平台的程序时，MVVM 架构可帮助开发人员将代码分离为平台特定的 View——即需要与平台 API 交互的代码——以及平台无关的 Model 和 ViewModel。

通常，这些平台无关的代码需要访问文件或网络，或者使用集合和线程。通常，这些工作会被视为操作系统 API 的一部分，但它们也可以利用 .NET Framework 类库来完成；如果该类库在每个平台上都可用，那么它实际上就是平台无关的。

应用程序中平台无关的部分随后可以被隔离出来，并且在 Visual Studio 或 Xamarin Studio 环境中放入一个单独的项目。这可以是共享资产项目（SAP）——它仅包含代码和其他可从其他项目访问的资产文件——也可以是可移植类库（PCL），它将所有公共代码封装在动态链接库（DLL）中，其他项目随后可以引用该 DLL。

无论使用哪种方法，这些公共代码都可以访问 .NET Framework 类库，因此它可以执行文件 I/O、处理全球化、访问 Web 服务、解析 XML 等等。

这意味着你可以创建一个包含四个 C# 项目的 Visual Studio 解决方案，以面向三大移动平台；或者你可以使用 Xamarin Studio 来面向 iPhone 和 Android 设备。

下图大致展示了 Visual Studio 或 Xamarin Studio 项目、Xamarin 库和平台 API 之间的相互关系：

![](img/c2d61d0459c51981af78607f1eef3d1c_17_0.png)

第二行的方框是实际的平台特定应用程序。它们会调用公共项目，并且（在 iPhone 和 Android 的情况下）还会调用实现原生平台 API 的 Xamarin 库。

但该图并不十分完整：它没有显示 SAP 或 PCL 调用 .NET Framework 类库的情况。PCL 可以访问其自身的 .NET 版本，而 SAP 使用集成到每个特定平台中的 .NET 版本。

在这个图中，Xamarin.iOS 和 Xamarin.Android 库看起来相当庞大，虽然它们确实重要，但它们大多只是语言绑定，并不会显著增加 API 调用的开销。

构建 iPhone 应用时，Xamarin C# 编译器像往常一样生成 C# 中间语言（IL），然后利用 Mac 上的 Apple 编译器生成本机 iPhone 机器代码，就像 Objective-C 编译器一样。应用对 iPhone API 的调用与使用 Objective-C 编写的应用完全相同。

对于 Android 应用，Xamarin C# 编译器会生成 IL，IL 在设备上与 Java 引擎并行运行的一个 Mono 版本上运行，但应用的 API 调用几乎与用 Java 编写的应用相同。

对于有非常平台特定需求、但也包含可能可共享的平台无关代码块的移动应用，Xamarin.iOS 和 Xamarin.Android 提供了出色的解决方案。你可以访问完整的平台 API，并拥有随之而来的全部能力（和责任）。

但对于可能不需要那么多平台特定性的应用，现在有一种替代方案，能让你的工作更加简单。

## Xamarin.Forms 简介

2014 年 5 月 28 日，Xamarin 推出了 Xamarin.Forms，作为名为 Xamarin 3 的 Xamarin 平台一系列增强功能的一部分。Xamarin.Forms 允许你编写可为 iPhone、Android 和 Windows Phone 编译的用户界面代码。

## Xamarin.Forms 选项

在一般情况下，Xamarin.Forms 应用程序是由三个面向三大移动平台的独立项目和一个包含公共代码的第四个项目组成——这与前一节中的图非常相似。

然而，在 Xamarin.Forms 应用程序中，这三个平台项目通常非常小——往往只包含带有少量样板启动代码的存根。共享资产项目或可移植类库项目包含应用程序的主体，包括用户界面代码：

![](img/c2d61d0459c51981af78607f1eef3d1c_18_0.png)

Xamarin.Forms.Core 和 Xamarin.Forms.Xaml 库实现了 Xamarin.Forms API。根据平台不同，Xamarin.Forms.Core 会使用相应的 Xamarin.Forms.Platform 库之一。这些库主要是一组称为渲染器的类，负责将 Xamarin.Forms 用户界面对象转换为平台特定的用户界面。

图表的其余部分与前面显示的相同。

例如，假设你需要前面讨论过的、允许用户在两个选项之间切换的用户界面对象。在使用 Xamarin.Forms 编程时，这被称为 `Switch`，在 Xamarin.Forms.Core 库中实现了一个名为 `Switch` 的类。在这三个平台的各个渲染器中，这个 `Switch` 被映射到 iPhone 上的 `UISwitch`、Android 上的 `Switch` 以及 Windows Phone 上的 `ToggleSwitchButton`。

Xamarin.Forms.Core 还包含一个名为 `Slider` 的类，用于显示一个水平条，用户通过操作它来选择数值。在平台特定库的渲染器中，它被映射为 iPhone 上的 `UISlider`、Android 上的 `SeekBar` 以及 Windows Phone 上的 `Slider`。

这意味着，当你编写一个包含 `Switch` 或 `Slider` 的 Xamarin.Forms 程序时，实际显示的是每个平台中实现的相关对象。

下面是一个简单的 Xamarin.Forms 程序，包含一个显示 “Hello, Xamarin.Forms!” 的 `Label`、一个显示 “Click Me!” 的 `Button`、一个 `Switch` 和一个 `Slider`。该程序正在（从左到右）iPhone、Android 和 Windows Phone 上运行：

![](img/c2d61d0459c51981af78607f1eef3d1c_19_0.png)

在本书中，你会反复看到这样的三联屏幕截图。它们的顺序总是相同的——iPhone、Android 和 Windows Phone——并且总是运行同一个程序。

## 机器与 IDE

如果你想开发 iPhone 应用，你将需要一台 Mac。Apple 要求必须使用 Mac 来构建 iPhone 和其他 iOS 应用程序。你需要在这台机器上安装 Xcode，当然还要安装包含所需库和 Xamarin Studio 的 Xamarin 平台。然后，你就可以在 Mac 上使用 Xamarin Studio 和 Xamarin.Forms 进行 iPhone 开发。

一旦你拥有一台安装了 Xcode 和 Xamarin 平台的 Mac，你也可以在 PC 上安装 Xamarin 平台，并使用 Visual Studio 为 iPhone 编程。PC 和 Mac 必须通过网络（如 WiFi）连接。你需要为这种互联在 Mac 上运行 Xamarin.iOS Build Host，Visual Studio 会利用它在 Mac 上构建并部署可执行文件。

虽然你可以在 PC 上运行 Xamarin Studio，但不能使用 PC 上的 Xamarin Studio 进行 iPhone 编程。

如果你想开发 Android 手机应用，你有很多灵活选择。你可以使用 Mac 上的 Xamarin Studio、PC 上的 Xamarin Studio 或 PC 上的 Visual Studio 来实现。

如果你想开发 Windows Phone 应用，你需要使用 Visual Studio 2012 或 2013，但不能使用 Express 版本。

这意味着，如果你想在单个 IDE 中面向所有三个平台，你可以在通过网络连接到 Mac 的 PC 上运行 Visual Studio 来实现。另一种选择是在 Mac 上的虚拟机中运行 Visual Studio。

## 设备与模拟器

你可以通过 USB 线将真实手机连接到机器来测试程序，也可以使用屏幕上的模拟器测试程序。

每种方法都有优缺点。真实手机对于测试复杂的触摸交互或感受启动或响应时间至关重要。但是，模拟器可以让你看到应用程序如何适应各种尺寸和外形。

运行最流畅的模拟器也许是 iPhone 模拟器。然而，由于 Mac 台式机没有触摸屏，你需要使用鼠标或鼠标垫来模拟触摸。Mac 触控板上的触摸手势不会传递到模拟器。你也可以将真实的 iPhone 连接到 Mac，但你需要将其配置为开发设备。

Windows Phone 模拟器支持多种不同的屏幕分辨率，并且往往运行得相当流畅，尽管会消耗大量内存。如果你在触摸屏上运行 Windows Phone 模拟器，你可以在模拟器屏幕上使用触摸。将真实的 Windows Phone 连接到 PC 需要解锁手机。如果你想解锁多部手机，你需要一个开发者帐户。

Android 模拟器的问题最多。它们往往又慢又不稳定，尽管在模拟大量真实 Android 设备方面常常非常灵活。另一方面，很容易

