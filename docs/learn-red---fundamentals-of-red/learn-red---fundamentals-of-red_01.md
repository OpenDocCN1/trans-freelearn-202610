# Red 的使命

在本章中，我们将了解 Red 的主要特征，更具体地说，是 Red 与其他编程语言相比的突出之处。这将激励你开始学习 Red。不要担心如果某些陈述在这个阶段有点抽象；当我们在接下来的章节开始使用 Red 时，它们会变得更清晰。回头再看这一章可能是一个好主意。

我们将涵盖以下主题：

* Red 的简史
* 使 Red 特别之处
* Red 的主要特性

# 技术要求




你可以在这里找到本章的代码：[`github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter01`](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter01)

# Red 的简短历史

Red 是一个开源的现代编程语言和平台，由 *Nenad Rakocevic* 设计和开发。Red 于 2011 年初开始，当前版本为 0.6.3（0.6.4 即将发布），目标是在未来一年左右发布 1.0 版本。为了更好地理解其特性，我们需要稍微探索一下它的名称和历史。

## Red 代表什么

Red 是 *Red\[uced\] REBOL* 的缩写，这就是为什么 Red 用户自豪地称自己为 *reducers*。这指出了两点：
* Red 建立在 REBOL 之上，REBOL 是一种在千禧年左右获得一定知名度的语言，由 Carl Sassenrath 构建。事实上，Red 在语法和语义上是 REBOL 的 95% 克隆，但在可能的情况下尝试改进和增强它。这也意味着 REBOL 脚本可以轻松转换为 Red。
* Red（像 REBOL 一样）希望减少当今的代码复杂性——它旨在拥有紧凑的运行时环境和依赖项（约 1 MB），而不是 Java 或 .NET 平台的数百 MB。

## 一些历史

Red 的开发始于 `Red/System`——这是一种低级语言（如 C），用于实现全系统级别的编程。`Red/System` 就像一个年长的兄弟——它看起来非常像 Red，但它针对的是更低级的编程，因此限制更多。我们将在 第 10 章 中再次讨论它，*Advanced Red*。

逐渐地，Red 在此基础上发展，每次发布都增加更多的数据类型、操作和函数。REPL 交互式编码控制台、在 ARM 处理器上执行、解析引擎、跨平台 GUI 方言（在 Windows 和 macOS 上运行，并在 Linux 和 Android 上接近完成）、响应式编程和宏都是这一演进的标志。

自 2015 年 1 月起，**Fullstack Technologies** 公司成立，以继续 Red 的持续开发，同时与不断增长的开源贡献者社区一起。该公司从 InnovationWorks 和 GeekFounders（中国风险投资早期投资者）筹集资金，总部位于北京的 **Silicon Valley**。

2018 年 1 月，Red 加入了区块链革命——通过 **Initial Coin Offering**（**ICO**）发行自己的加密货币 RED（Red Community Token）代币，并获得了大量新的资金以维持和加速其完成。利用其独特功能，名为 `Red/C3` 的特殊 Red 方言正在构建中，专门用于区块链和智能合约编程。

自 2018 年春季以来，Red 项目由位于巴黎（法国）的 **Red Foundation** 管理，并由 Nenad Rakocevic 领导。该组织还将促进开源项目的新经济模式，利用 RED 代币推动开发。

## Red 的特别之处

这里我们集中讨论使 Red 与其他当代编程语言区别开来的特性。

### 全栈语言——从底层到元层

与其基础 `Red/System` 结合，Red 形成了一种 *full-stack language*——这意味着它可用于开发高级应用程序（领域特定语言、图形前端和高层脚本）以及低级程序（嵌入式系统、操作系统和设备驱动程序），如下图所示，与其他语言相比：

![](img/64b85ca6-2267-428f-90a3-44bfebe72af4.png)

在编程历史上，一个语言覆盖整个编程栈是相当独特的。这意味着作为 Red 开发人员，你只需掌握一种语言即可进行从低级到高级的编码！

### 代码即数据，数据即代码

Red 是久负盛名的 LISP 编程语言的现代后裔。Red 采用了 Logo 的简化语法，而不是 LISP 笨重的括号用法。LISP 中的一切都是一种 *LIS*t *P*rocessing，Red 也是如此——基本上所有代码都是对 *blocks* 的操作组合，而 *blocks* 是 Red 中相当于 LISP 列表的东西。

一个 block 只是 `\[ \]` 方括号之间的一组值，例如 `\[7 13 42\]`。你很快就会看到，blocks 是 Red 代码中的基本构建单元。这导致仅使用最少的语法来表示代码和数据。

你会发现 `\[ \]` 在 Red 中被大量使用。在英语或美国键盘上这很容易，有专门的 \[ 和 \] 键。在我的（荷兰）键盘上，它是 *ALTGR* \+ *^* 和 *ALTGR* + *$*。在某些键盘上，它是 *Alt* + *5* 和 *Alt* + *6*。我发现编码时切换到美国键盘布局更容易。

像 LISP 一样，它具有非常强大的能力——它是 *homoiconic*，字面意思是 **self-representing**。这意味着 Red 可以像处理数据值一样处理其代码，因为数据和代码以相同的方式表示，即作为 blocks。这使得程序可以编写其他程序，换句话说，就是进行 *metaprogramming*。我们将在后面的章节中看到该特性的几个示例。

### 内置数据类型

为了让开发者高效地处理数据，Red 拥有 45 多种（且不断增加）的内置数据类型，从常见的整数、字符和字符串到不太常见的类型，如 pairs、tuples、文件、URL、电子邮件等。此外，你使用同一组操作来处理它们，这大大降低了学习曲线。要在其他语言中做到这一点，你必须从专用类创建对象才能开始使用它们，甚至还要导入库。作为第一个例子，你可以简单地按如下方式为日期值加减天数：

```
;-- see Chapter01/special.red:
1-Feb-2018 + 14         ; == 15-Feb-2018
1-Feb-2018 – 42         ; == 21-Dec-2017
```

### 小型工具链和可执行文件

正如我们在下一章设置时将看到的，完整的工具链（编译器、标准库、文档和 REPL）打包在一个最小的 1 MB 文件中，具有 *no installation and configuration requirements*——这里我们可以看到简化正在发挥作用！

将其与约大 150 倍的 Java 运行时，或也大 50 倍的 Python 运行时进行比较。

编译后，Red 生成的可执行文件大小通常约为 0.5 到 1 MB，且无需依赖。这极大地简化了应用程序的生产部署。这一方面加上较低的内存占用，使 Red 成为移动和嵌入式应用程序的非常有吸引力的选择。一个垃圾收集器将在下一个版本之一中集成，确保内存消耗保持较低。

Red 编程非常富有表现力且简洁，减少了脚本大小，范围在 KB 级。这将在后面的章节中变得明显。例如，要将网页读入字符串 `page`，你可以这样做：

```
page: read http://www.red-lang.org
```

2013 年，Redmonk 网站发布了一篇文章（你可以在这里查看：[`redmonk.com/dberkholz/2013/03/25/programming-languages-ranked-by-expressiveness/`](http://redmonk.com/dberkholz/2013/03/25/programming-languages-ranked-by-expressiveness/)）考察了语言的表现力。结果在下图中进行了总结：

![](img/2d03d09b-f3c9-4541-8ba9-3d6ddbcb81b6.png)

在垂直轴上，描绘了每次提交的代码行数。我们看到 REBOL（Red 的直接父语言）与主流语言相比得分非常低，表明编码可比任务所需的行数更少。

## 方言



One of Red's greatest strengths is the ability to easily create **领域特定语言** (**DSLs**) or **方言**。These are small and optimized sub-languages designed for a specific task, such as `parse` for transforming text (see 第 8 章, *解析数据*), `view` and `draw` for composing graphical user interfaces (see 第 9 章, *组合视觉界面*), and `Red/C3` for blockchain programming (see 第 10 章, *高级 Red*).

# Red 的主要特性

As a programming language, Red embraces nearly all paradigms:

*   **命令式**: Providing everything needed for controlling code execution and error-handling (see 第 4 章, *代码控制结构* and 第 5 章, *使用序列和块*)

*   **函数式**: With functions as first class values (see 第 6 章, *使用函数和对象*)

*   **面向对象**: Complete with inheritance, but not class-based (see 第 6 章, *使用函数和对象*)

*   **并发**: With async task support and an actor model (from v 0.9.0)

*   **符号式**: Being able to manipulate its own code as if it was plain data (all chapters)

*   **响应式**: Included in the **GUI** (**图形用户界面**) system (see 第 9 章, *组合视觉界面* and 第 10 章, *高级 Red*)

Aside from REBOL and LISP, Red has taken inspiration from the following:

*   Forth and Logo for its syntax

*   Lua, for its capabilities as an embedded language, and its **JIT** (**即时**) compiler

*   Self, for its prototype object-model

*   Scala, for its type inference engine and compilation

Although it can be statically compiled, Red has a graphical **REPL** console (**读取求值打印循环**) like other dynamic and scripting languages, to experiment with code interactively. We will explore this console in 第 2 章, *设置开发环境*.

In contrast to REBOL, which is an interpreted language only, Red also *compiles to native code* through `Red/System`, thus achieving much better performance, something like Crystal compared to Ruby. Compared to C, at this moment `Red/System` is 2-4x slower.

## 可移植性

Red runs in the Windows, Linux, Android, macOS, and FreeBSD environments in a 32-bit version (the 64-bit version is planned) with cross-compilation capability. Furthermore, it runs on ARMv5 processors and on the Raspberry Pi and Arduino boards.

## 如同瑞士军刀

The versatility of the Red platform makes it suitable for apps on the entire spectrum, such as:

*   High-level scripting (for example, glue code such as Lua)

*   Web apps

*   GUI desktop applications

*   Android apps (from Version 0.7 onward)

*   2D games

*   Data processing

*   Grammar parsing and constructing specialized languages

*   Network/system programming, IoT devices programming, robotics

## 摘要

In this chapter, we looked at Red's main characteristics, and examined its place among other programming languages. In the following chapter, we will set up an environment to start up our development with Red.

## 问题

1.  Why does Red have a `Red/System` component?

2.  What is a full-stack language, and why is Red an example of that?

3.  What do the the Red and LISP languages have in common?

4.  Give three ways in which Red reduces software complexity.

5.  What is a dialect in Red? Give some examples.

6.  Why is Red faster than REBOL?

7.  Name some of the environments in which Red can run.
