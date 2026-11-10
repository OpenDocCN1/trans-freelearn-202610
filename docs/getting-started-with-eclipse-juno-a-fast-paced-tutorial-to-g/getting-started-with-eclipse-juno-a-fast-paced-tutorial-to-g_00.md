# Eclipse Juno 入门指南：Eclipse Juno IDE 快速上手教程

## 致谢

**作者**
Rodrigo Fraxino Araujo
Vinicius H. S. Durelli
Rafael M. Teixeira

**校对编辑**
Insiya Morbiwala
Aditya Nair

**项目协调员**
Anugya Khurana

**审阅者**
Francisco Osorio Gálvez
Peter Rice

**校对员**
Clyde Jenkins

**收购编辑**
Amarabha Banerjee
Rukhsana Khambatta

**索引**
Tejal R. Soni

**主技术编辑**
Ankita Shashi

**生产协调员**
Conidon Miranda

**技术编辑**
Sharvari H. Baet
Athira Lalji
Veena Pagare

**封面工作**
Conidon Miranda

## 关于作者

**Rodrigo Fraxino Araujo** 是巴西圣保罗大学计算机科学专业的博士候选人。他曾在法国 Rocquencourt 的国家信息与自动化研究所担任一年访问学者。他也是软件工程师，自 2011 年起在 IBM 工作。

> 我特别想感谢 Carolina，她给了我完成这本书的动力。

**Vinicius H. S. Durelli** 是巴西圣保罗大学计算机科学专业的博士候选人。作为博士学习的一部分，他于 2011 年至 2012 年在美国弗吉尼亚州乔治梅森大学担任访问学者。他于 2008 年获得圣卡洛斯联邦大学计算机科学硕士学位。他自 2006 年起持有 Sun 认证 Java 程序员资格。此外，他自约 2004 年起使用 Eclipse，这让他感觉自己老了。当他不写代码或编程时，Vinicius 喜欢玩电子游戏（尤其是马里奥和塞尔达游戏）并练习巴西柔术。

> 我想对我的朋友 Simone Borges 表示最深切的感谢，她在我努力赶上与这本书和我正在进行的博士学习相关的截止日期时，总是把食物放在我面前让我坚持下去。

**Rafael M. Teixeira** 目前在 IBM Linux 技术中心担任软件工程师，为 Eclipse 开源项目开发代码。他目前正在圣保罗大学攻读硕士学位，也是在那里获得计算机工程学位。Rafael 最喜欢的爱好是跑步，但也偶尔喜欢玩电子游戏。

> 我想感谢我的家人给予的关爱和支持，这使一切成为可能。我还要感谢 Bianca Teixeira，她激励人心且甜美的言语在过去 7 年里一直激励着我，希望在未来许多年里也能继续如此。我将此作品献给 Luis Fernando Teixeira 的记忆。

## 关于审阅者

**Francisco Osorio Gálvez** 是 Citigroup 的高级开发人员和技术负责人，此前曾在 IBM 工作。他从事为银行业设计、开发和领导大型 Java 企业系统的设计、开发和领导工作。他喜欢仅仅为了好玩而使用不同的技术。您可以通过 `fosoriog@gmail.com` 联系他。

> 我想感谢我的妻子 Fabiola，感谢她在我审阅这本书时牺牲了属于她的时间。周末都被牺牲了。我会补偿你的，我保证。 ;)

**Peter Rice** 是美国佐治亚大学数学系的荣誉退休教授。退休后他开始了 IT 行业的职业生涯，这使他成为微软认证培训师、IBM 培训师，并通过自己的公司 PM Rice Consulting, Inc. 进行了大量独立培训工作。此外，他还创建并担任了多项商业程序的主要架构师。他还使用多种语言工作。

## www.PacktPub.com

## 支持文件、电子书、折扣优惠等

您可能想访问 www.PacktPub.com 获取与本书相关的支持文件和下载。

您知道 Packt 提供每本出版书籍的电子书版本，包含 PDF 和 ePub 文件吗？您可以在 www.PacktPub.com 升级到电子书版本，作为印刷书籍客户，您有权享受电子书的折扣。欲了解更多详情，请联系 service@packtpub.com。

在 www.PacktPub.com，您还可以阅读大量免费技术文章，订阅多种免费新闻通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/d7c3f2018603810fae83e1a6fd98749e_7_0.png)

http://PacktLib.PacktPub.com

您需要即时解决 IT 问题吗？PacktLib 是 Packt 的在线数字图书库。在这里，您可以访问、阅读和搜索 Packt 整个图书库。

## 为什么订阅？

- 在 Packt 出版的每本书中完全可搜索
- 复制和粘贴、打印和书签内容
- 按需访问并通过网络浏览器访问

## Packt 账户持有者的免费访问

如果您在 www.PacktPub.com 有 Packt 账户，您可以使用此账户立即访问 PacktLib 并查看九本完全免费的书籍。只需使用您的登录凭据即可立即访问。

## 前言

第一个版本的 Eclipse 于 2001 年 11 月发布。自那时起，无论在增强功能还是用户数量方面，它都取得了长足的进步。由于经历了许多改进，Eclipse 随着时间的推移变得流行并获得了大量赞誉。如今，Eclipse 无疑是用于开发 Java 应用程序的最广泛使用的集成开发环境（IDE）。它免费且开源，并附带用于执行大量与软件开发相关活动的插件。此外，还可以根据个人需求和偏好轻松下载和安装更多插件。另一个优点是，由于 Eclipse 大部分是用 Java 编写的，因此它是跨平台的。

如果你一直在使用没有 IDE 帮助的 Java，你现在一定已经意识到 Java 有时可能很粗糙且令人不安。大多数时候，编译器（javac）不会显示有用的错误信息。即使是像遗漏分号这样简单的事情，也可能导致它显示冗长的错误信息列表。在 IDE 内部，这类问题可以在你尝试运行程序之前被发现。IDE 甚至可以在你开始抓头之前就建议解决方案。毫无疑问，使用功能齐全的 IDE 提供了各种好处。例如，IDE 提供代码自动完成，它帮助你正确格式化代码，其中一些还附带用于测试、调试以及跟踪你对代码所做的更改的精巧插件。

然而，尽管使用 IDE 有诸多好处，我们还是要坦率地说：学习如何使用这些高端开发工具绝非易事。IDE 具有陡峭的学习曲线，这意味着掌握此类工具通常需要时间和精力。更糟糕的是，有时许多开发人员会同时尝试学习一种新的编程语言和一个 IDE。

本书将帮助你克服 Eclipse 陡峭的学习曲线。阅读本书后，你将能够熟练使用 Eclipse 执行许多开发活动。

## 本书涵盖内容

本书涵盖了 Eclipse Juno，即在撰写本文时 IDE 的最新稳定版本（于 2012 年 6 月发布）。本书首先展示如何执行与实现 Java 应用程序相关的最基本活动（如创建和组织 Java 项目、重构以及设置启动配置），逐步深入到更复杂的主题，如测试、Web 开发、GUI 编程以及扩展 Eclipse 编辑器以满足你的需求。

本书采用实践方法，直接深入使用 Eclipse，而不会让你陷入技术细节的泥潭。以下是本书涵盖的一些主题：

- 有效使用 Eclipse 编写 Java 代码
- 将 JUnit 4 集成到 Eclipse 中，开发和运行基于 JUnit 的测试方法
- 使用版本控制系统 Git/EGit 管理你的项目
- 使用 SWT 开发 GUI 应用程序
- 带有示例的高级 GUI 开发
- 使用 Eclipse WTP 插件开发和部署基于 Web 的 Java 应用程序
- 通过开发新编辑器、视图以及为 IDE 的现有元素做出贡献来扩展 Eclipse

第 1 章，简介，介绍了如何开始使用 Eclipse。它从如何启动 Eclipse 到对 Eclipse Juno 的新功能和值得注意功能的简要概述。

第 2 章，Java 开发，涵盖使用 Eclipse 进行基本的 Java 开发。对于初学者来说，Eclipse 可能相当令人望而生畏。主窗口包含许多较小的窗口，其中一些较小的窗口有许多按钮和下拉菜单。本章的全部内容是了解 Eclipse 用于开发 Java 程序的主要功能。我们将逐步介绍从创建 Java 项目开始的所有步骤，详细说明用于 Java 开发的 Eclipse 视图、Java 代码生成，到构建和运行 Java 项目，这涉及设置启动配置。此外，我们还涵盖了一些重构，以便你能够改进 Java 代码——当你处理更复杂的项目时，你会发现重构是 Java 开发中非常重要的一方面。

第 3 章，使用 JUnit 进行单元测试和调试，介绍了 JUnit，即用于在 Java 中实现单元测试的最广泛使用的框架。你将学习如何设置 JUnit，以便它可以与 Eclipse 结合使用。此外，本章还描述了使用 JUnit 注解创建测试方法。正如你将看到的，注解还可以用来告知 JUnit 何时跳过特定的测试方法以及给定方法是否预期抛出异常。本章以介绍如何使用 Eclipse 的调试器来精确定位代码中难以找到的问题作为结尾。

第 4 章，版本控制系统，解释了如何将 Eclipse 与版本控制系统结合使用。当处理复杂的软件项目（或涉及多人的项目）时，你需要使用一个管理并跟踪相关软件项目不同版本的工具。本书涵盖了 CVS、SVN 和 Git，以及如何通过原生和外部插件在 Eclipse 内部使用它们。阅读本章后，你将能够在 Eclipse 内部使用它们来支持协作开发。

第 5 章，SWT，涵盖使用标准部件工具包（SWT）进行图形用户界面（GUI）开发。本章介绍了最基本的部件，例如按钮、标签和组合框。从某种意义上说，本章为第 6 章《更多 SWT》奠定了基础。

第 6 章，更多 SWT，在前一章的基础上构建，展示了如何通过引入高级部件（如工具栏和表格）来创建更具吸引力的 GUI。本章还包含一个完整的示例。

第 7 章，使用 Eclipse WTP 进行 Web 开发，关注使用 Eclipse 开发基本 Web 应用程序。在本章中，我们展示了 Eclipse 如何与一组名为 Web Tools Platform（WTP）的插件一起使用，以支持基于两种基本 Web 技术——即 servlet 和 JavaServer Pages（JSP）的 Web 应用程序的开发。

第 8 章，Eclipse 开发，涵盖一个更高级的主题：扩展 Eclipse。你将学习 Eclipse 的架构是如何组织的。此外，通过阅读本章，你将看到如何利用扩展点来创建自己的插件、编辑器，甚至扩展特定的视图。



第9章《Eclipse Rich Client Platform》展示了如何利用上一章学到的知识，使用 Eclipse Rich Client Platform 开发自己的富客户端应用程序。通过创建示例应用程序，您将学习如何添加视图、菜单和面板，以及使它们相互交互。我们还将介绍如何为应用程序品牌化和打包，让您能够使用 Eclipse 平台构建功能齐全的跨平台应用程序。

附录《Keyboard Shortcuts》包含了书中介绍的所有键盘快捷键，以及其他能够大大提高您 Eclipse 生产力的实用快捷键。

## 本书所需
## 本书所需

本书所需的两款软件是 Java 虚拟机（JVM）和 Eclipse。两者都是免费的，可供下载。还需要一些额外的插件和软件，但我们将在全书中详细介绍如何获取和安装它们。下一节详细介绍了如何安装 Eclipse，并提供了有关如何安装 JVM 版本的信息来源提示。

## 本书适合谁阅读
## 本书适合谁阅读

尽管 Eclipse 可用作多种语言的 IDE，但它在 Java 社区中更受欢迎。我们撰写本书时以对充分利用 Eclipse 感兴趣的 Java 程序员为目标。因此，主要重点将是使用 Eclipse 进行 Java 开发，本书的主要读者是 Java 程序员。

本书的编写方式使其既适合初学者，也适合中级 Java 程序员。熟悉 Java 的研究生也会从本书中受益。实践中的软件开发人员可能会将本书作为进入 Eclipse 世界的首次尝试。如果您是一位经验丰富的 Java 开发人员，一直在使用其他 IDE 并想知道 Eclipse 有何优势，本书将为您提供主要 IDE 功能的实践指导。经验丰富的 Eclipse 用户可能不会从本书中受益，因为本书旨在作为入门介绍。换句话说，本书可以被视为掌握 Eclipse 的第一步；它是一份快节奏的教程，适合那些希望立即上手并开始高效使用 Eclipse 的人。

如果您不熟悉 Java，那么这本书不适合您。如果您刚刚接触 Java，在进行一次 Java 语法速成课程后，您将能够跟上本书的大部分内容（只要确保手边有一本 Java 教科书）。总体而言，这里展示的 Java 代码片段对 Java 初学者来说是可理解的。我们尽量让示例中的代码尽可能简单，以便初学者不会被 Java 语法相关的技术细节困住。每当出现稍微复杂一点的内容时，我们都会提供概述或引导读者阅读该主题的更深入讨论。

## 惯例
## 惯例

本书中您会发现多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义说明。

Code words in text are shown as follows: " The option to transform a for loop into an indexed for loop is available for expressions."
正文中代码词的显示方式如下："可以将 for 循环转换为索引 for 循环的选项可用于表达式。"

A block of code is set as follows:
代码块的设置方式如下：

```
@Test(timeout=1000)
public void testSquareRoot() {
    while(true);
}
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:
当我们希望引起您对代码块中特定部分的注意时，相关行或项目会以粗体显示：

```
@Test(timeout=1000)
**public void testSquareRoot() {**
    while(true);
}
```

**New terms** and **important words** are shown in bold. Words that you see on the screen, in menus or dialog boxes for example, appear in the text like this: " We strongly recommend you to download the **Eclipse IDE for Java Developers**.
**新术语**和**重要词语**以粗体显示。例如，您在屏幕、菜单或对话框中看到的词语，会在正文中这样出现："我们强烈建议您下载**Eclipse IDE for Java Developers**。

[  Warnings or important notes appear in a box like this.  ]
[  警告或重要说明会以这样的方框出现。  ]

[  Tips and tricks appear like this.  ]
[  技巧和窍门会这样出现。  ]

## 读者反馈
## 读者反馈

我们始终欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢或可能不喜欢的部分。读者的反馈对我们开发您真正受益的书目非常重要。

To send us general feedback, simply send an e-mail to `feedback@packtpub.com`, and mention the book title via the subject of your message.
如需发送一般反馈，只需发送电子邮件至 `feedback@packtpub.com`，并在邮件主题中提及书名即可。

If there is a book that you need and would like to see us publish, please send us a note in the **SUGGEST A TITLE** form on `www.packtpub.com` or e-mail `suggest@packtpub.com`.
如果您需要某本书并希望我们出版，请在 `www.packtpub.com` 上的 **建议书名** 表单中留言，或发送电子邮件至 `suggest@packtpub.com`。

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide on `www.packtpub.com/authors`.
如果您在某个主题上有专长并有兴趣撰写或为图书做出贡献，请参阅 `www.packtpub.com/authors` 上的作者指南。

## 客户支持
## 客户支持

现在您已成为 Packt 图书的自豪拥有者，我们提供多项帮助，让您充分利用您的购买。

## 下载示例代码
## 下载示例代码

You can download the example code files for all Packt books you have purchased from your account at `http://www.PacktPub.com`. If you purchased this book elsewhere, you can visit `http://www.PacktPub.com/support` and register to have the files e-mailed directly to you.
您可以从 `http://www.PacktPub.com` 上的账户下载您购买的所有 Packt 图书的示例代码文件。如果您在其他地方购买了本书，可以访问 `http://www.PacktPub.com/support` 并注册，以便将文件直接通过电子邮件发送给您。

## 勘误
## 勘误

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books – maybe a mistake in the text or the code – we would be grateful if you would report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting `http://www.packtpub.com/support`, selecting your book, clicking on the **errata submission form** link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded on our website, or added to any list of existing errata, under the Errata section of that title. Any existing errata can be viewed by selecting your title from `http://www.packtpub.com/support`.
尽管我们已尽一切努力确保内容的准确性，但错误仍可能发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。这样做可以避免其他读者感到沮丧，并帮助我们改进后续版本的书籍。如果您发现任何勘误，请访问 `http://www.packtpub.com/support`，选择您的图书，点击 **勘误提交表单** 链接并输入勘误详情。一旦您的勘误得到验证，您的提交将被接受，勘误将被上传到我们的网站，或添加到该书标题的勘误部分中现有勘误列表中。任何现有勘误都可以通过从 `http://www.packtpub.com/support` 选择您的书名来查看。

## 盗版
## 盗版

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works, in any form, on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.
互联网上版权材料的盗版是所有媒体持续存在的问题。在 Packt，我们非常重视版权和许可的保护。如果您在互联网上发现我们作品的任何非法复制品，无论形式如何，请立即提供位置地址或网站名称，以便我们采取补救措施。

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.
请通过 copyright@packtpub.com 联系我们，并提供涉嫌盗版材料的链接。

We appreciate your help in protecting our authors, and our ability to bring you valuable content.
我们感谢您帮助保护我们的作者，以及我们为您带来有价值内容的能力。

## 问题
## 问题

You can contact us at questions@packtpub.com if you are having a problem with any aspect of the book, and we will do our best to address it.
如果您在本书的任何方面遇到问题，可以联系我们 questions@packtpub.com，我们将尽力解决。
