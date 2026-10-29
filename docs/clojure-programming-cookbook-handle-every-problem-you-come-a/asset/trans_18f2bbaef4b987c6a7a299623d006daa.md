

// Comment to get more information during initialization
//logLevel := Level.Debug
logLevel := Level.Info
//lazy val root =
project.in(file(".")).dependsOn(clojurePlugin)

//lazy val clojurePlugin = file("../../sbt-clojure")
addSbtPlugin("com.unhandlederexpression" % "sbt-clojure" % "0.1")

## 编写源代码

我们将在 src/main/clojure 下创建 Clojure 源代码：

```
(ns helloworld)
(gen-class
  :name cljbook.chapter05.HelloWorld
  :methods [#^{:static true} [sayHello [] void]])
(defn -sayHello []
  (println "Hello world from Clojure!"))
(gen-class
  :name cljbook.chapter05.Calculator
  :methods
  [#^{:static false} [add [long long] long]
   #^{:static false} [sub [long long] long]])
(defn -add [this a b]
  (+ a b))
(defn -sub [this a b]
  (- a b))
```

前面的 Clojure 代码有两个类：
cljbook.chapter05.HelloWorld 和 cljbook.chapter05.Calculator。
然后，我们将查看 Scala 代码。以下是调用 Clojure 中定义的类的 Scala 代码：

```
import cljbook.chapter05.HelloWorld
import cljbook.chapter05.Calculator
object Main {
  def main(args: Array[String]) = {
    println("I'm calling methods defined by Clojure from Scala.")
    HelloWorld.sayHello();
    val calc = new Calculator
    println("add: "+calc.add(1,2))
    println("sub: "+calc.sub(10,5))
  }
```

以下是此项目的目录截图：

![](img/d7b631352a782ee50160cf8eb995152f_324_0.png)

然后运行以下命令：

```
$ sbt run
```

![](img/d7b631352a782ee50160cf8eb995152f_324_1.png)

## ClojureCLR

ClojureCLR 是一种将你的 Clojure 代码使用和移植到 .NET 运行时的途径。实现这种集成有不同层次；你希望在 CLR 上运行 Clojure 代码的原因将成为你使用方式及其不同模式的根源。你可能只是偶然发现了一些看起来很棒的 Clojure 代码，但由于那些括号，你不确定是否能调试它，所以你只想嵌入 clj 算法并能够在命令行或通过 Windows 原生界面运行它。你可能想从通常的 Clojure REPL 创建 Windows 用户界面，这也是使用 ClojureCLR 可以实现的。你可能还想将你的 Clojure 代码分发到 Windows 动态链接库 DLL 中并交给客户，以便他可以原生调用它。

在本教程中，我们将查看所有这些不同的选项，并学习如何在 CLR 上保持生产力。

在完成本教程的过程中，你还会再次意识到，.NET 程序的内存占用通常低于其 Java VM 对应程序，这为在内存受限环境中运行 Clojure 提供了很好的论据。

编写本教程时，作者很想写更多 C# 和其他相关的 .NET 代码，甚至写一些移动应用。现在轮到你兴奋了。

## 准备工作

实际上，要为本教程做好准备，我们需要做很多工作。另外，请确保不要在网上到处查找并尝试将其他网络来源的说明与本教程结合起来。随着时间的推移，安装和集成方面已经有了很大的改进，因此没有必要回到过时的说明。

首先，在 Windows 上，建议你安装 Visual Studio。

### Note

本教程中的所有示例都是使用 Community Edition 编写的。其中相当多的截图也来自那里。

不需要特定的插件，但本章中的示例将使用一些 C# 示例。

在过程中，我们还将查看如何集成来自 Xamarin Studio 的 mono IDE。Mono 允许你在非 Windows 机器上运行 .NET 代码；因此，你不需要运行仅限 Windows 的 Visual Studio，而可以在任何地方运行你漂亮集成的 .NET 和 Clojure 代码。

## 安装 Visual Code Studio 并创建项目

除了所需的磁盘空间外，这应该很简单，它占用了你宝贵 SSD 驱动器上几乎 10GB 的空间：

### Visual Studio

Community 2015
with Update 1

Features   Languages

Select features

- Programming Languages
- Windows and Web Development
  - ClickOnce Publishing Tools
  - Microsoft SQL Server Data Tools
  - Microsoft Web Developer Tools
  - PowerShell Tools for Visual Studio [3rd Party]
  - Silverlight Development Kit
  - Universal Windows App Development Tools
  - Windows 8.1 and Windows Phone 8.0/8.1 Tools
  - Cross Platform Mobile Development
  - Common Tools

Select All

Reset Defaults

Setup requires up to 10 MB across all drives.

Back

UPDATE

安装完成后，你将创建一个新的基于命令行的 C# 项目，如以下截图所示：

![](img/d7b631352a782ee50160cf8eb995152f_328_0.png)

创建新项目会带你进入一个非常熟悉的设置。为了准备好使用 Clojure 使用我们的项目，我们将使用 NuGet 包管理器，它是 .NET 中安装 Clojure 库的事实标准：

![](img/d7b631352a782ee50160cf8eb995152f_329_0.png)

现在我们将 Clojure 库安装到当前项目中。在 NuGet 控制台中，输入以下内容：

```
Install-Package Clojure -Version 1.7.0
```

现在 NuGet 会为你抓取基本的东西：

![](img/d7b631352a782ee50160cf8eb995152f_329_1.png)

这里有一点说明：ClojureCLR 版本 1.8.0 在本书出版前无法成功测试，所以我们坚持使用稍旧的版本。对于我们的目的来说，这不会有问题。

你现在可以从你的 C# 项目中执行你的第一个 Clojure 代码片段。

让我们在主应用程序核心中加入一些通常的互操作代码：

```
using System;
using clojure.lang;
using System.Threading;
using clojure.clr.api;

namespace firstclojure
{
class Program
{
    {
        static void Main(string[] args)
        {
            IFn load = clojure.clr.api.Clojure.var("clojure.core",
"load-string");
            Console.WriteLine(Convert.ToString(load.invoke("(+ 1 1)")));
            Thread.Sleep(1000);
        }
    }
}
```

执行前面的代码，并假设安装顺利，以下内容将会显示：

2

这意味着 Clojure 代码已使用 load-string 方法有效编译，并且已被 eval。最后我们多睡了一会儿，这样我们就有时间看到消息。值得注意的是，Objects explorer 窗口显示了对核心 Clojure 库的引用：

![](img/d7b631352a782ee50160cf8eb995152f_331_0.png)

浏览层次结构，我们可以看到可用的不同方法，而且 Visual Studio 在你输入代码时确实很好地为你显示选项。

这非常基础，我们稍后将看到如何做更多与 Clojure 的交互。

现在，由于工作流程非常相似，让我们看看在使用 Mono Studio 时如何在平台无关环境中做好准备。

## 安装 Mono Studio IDE

对于我们这些没有机会在 Windows 上工作的人来说，我们可以改用安装 Xamarin Studio。会有一些更多的体操，所以如果你在 Windows 上，坚持使用 Visual Studio 的舒适度可能是更好的选择：

![](img/d7b631352a782ee50160cf8eb995152f_332_0.png)

# Welcome to the Xamarin Studio 5.10.1 Setup Wizard

The Setup Wizard allows you to change the way Xamarin Studio 5.10.1 features are installed on your computer or to remove it from your computer. Click Next to continue or Cancel to exit the Setup Wizard.

设置完成后，新的项目流程应该感觉相似；你可以创建一个新的基于命令行的项目：

![](img/d7b631352a782ee50160cf8eb995152f_332_1.png)

# Choose a template for your new project

- Cross-platform
  - App
  - Library
  - Tests
- Other
  - .NET
  - ASP.NET
  - Miscellaneous

- Console Project
- Empty Project
- Gtk# 2.0 Project
- Library
- NUnit Library Project
- F# Tutorial

Console Project
Creates a new C# console project.

