# 基于Jakarta EE的云原生开发与迁移：将传统Java EE项目转变为云原生应用程序

# Contributors

# About the authors

**Ron Veen** is a highly experienced software engineer, navigating the spectrum from Midrange Systems to Micro Services. Driven by his passion for software engineering and software architecture, he has amassed more than 15 years of experience working on the JVM and the Java ecosystem. Ron has seen all the frameworks and libraries from Apache to ZK and has worked with many versions of Java EE and later, Jakarta EE. An avid fan of alternative JVM languages like Groovy, Scala, Clojure, and his personal favorite Kotlin, Ron is also an Oracle Certified Java Programmer (OCP) and Sun Certified Business Component Developer (SCBCD/OCPBCD). Ron is also a regular speaker at international conferences.

**David Vlijmincx** is a software developer with 8 years of experience in the field. He has a strong background in software development, with a focus on building scalable, high-quality applications using the Java programming language. David’s expertise spans through a variety of projects, ranging from small, standalone applications to large, complex systems, which needed to be migrated to newer versions of the EE specification or the cloud. David is an Oracle Certified Java Programmer (OCP) and is an avid blogger and speaker at industry conferences.

# About the reviewer

**Edwin Derks**, a distinguished Java Champion hailing from the Netherlands, excels in unraveling intricate and strategic IT challenges as a dedicated Consultant. He is passionate about gathering and sharing knowledge on various facets of the Java ecosystem, composable architectures, and cloud-driven development. He actively contributes to MicroProfile and Jakarta EE and often speaks at conferences, passionately sharing his knowledge and experience.

In his spare time, he is a loving husband and a father of three. He can often be found in the gym or having a good time at dance parties or heavy metal concerts.

第3章“从Java EE迁移到Jakarta EE”描述了如何着手从Java EE迁移到Jakarta EE。

第4章“使用最新特性现代化你的应用程序”概述了Jakarta EE 10的重大变化。本章还将向你展示如何实现弹性和监控等云原生特性。最后，我们将向你展示如何可视化来自应用程序的指标。

第5章“让你的应用程序可测试”概述了在迁移软件项目时测试为何重要，以及如何衡量测试。你将学习如何为现代应用程序编写单元测试和集成测试，以确保应用程序按预期持续工作。

第6章“容器与Docker简介”概述了什么是容器以及它们为何是重要工具。你将了解Docker是什么、如何安装它，以及如何基于Cargo Tracker应用程序创建Docker容器。

第7章“认识Kubernetes”向你介绍Kubernetes——运行容器的事实标准。

第8章“什么是云原生？”描述了云原生计算的原则。

第9章“在云中部署Jakarta EE应用程序”概述了将应用程序部署到Azure云的过程。你将学习如何创建容器注册表，以及如何从注册表中的容器创建实例。本章还介绍了如何查看已部署实例的指标。

第10章“MicroProfile简介”介绍了MicroProfile规范中最重要的规范。

附录A“Java EE到Jakarta EE名称”是Java EE到Jakarta EE规范的列表。

附录B“即服务”解释了利用互联网进行计算的不同概念。

## 充分利用本书

为了获得最佳学习效果，具备使用Java EE、Jakarta EE或Spring等企业框架开发Java应用程序的经验是必不可少的。

| 本书涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| Azure云 | Windows、macOS或Linux |
| Java 17 |  |
| Java EE 7 |  |
| Jakarta EE 10 |  |

如果你使用的是本书的数字版本，建议你自己输入代码，或从本书的GitHub仓库获取代码（链接见下一节）。这样做将有助于避免因复制粘贴代码而可能产生的任何错误。

## 下载示例代码文件

你可以从GitHub上的`https://github.com/PacktPublishing/Cloud-Native-Development-and-Migration-to-Jakarta-EE`下载本书的示例代码文件。如果代码有更新，它将在GitHub仓库中更新。

我们还有来自丰富的图书和视频目录的其他代码包，可在`https://github.com/PacktPublishing/`获取。去看看吧！

## 使用的约定

本书通篇使用了许多文本约定。

**文本中的代码：** 表示文本中的代码、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟URL、用户输入和Twitter句柄。这里有一个例子：“`param-value`告诉PrimeFaces，我们想为项目使用omega主题。”

一个代码块设置如下：

```xml
<context-param>
    <param-name>primefaces.THEME</param-name>
    <param-value>saga</param-value>
</context-param>
```

当我们希望引起你对代码块特定部分的注意时，相关行或项目会以粗体显示：

任何命令行输入或输出都如下所示：

```bash
mvn compile
```

**粗体：** 表示新术语、重要词语或你在屏幕上看到的文字。例如，菜单或对话框中的词语会以**粗体**显示。这里有一个例子：“如果可以应用快速修复，你可以通过右键单击事件并选择**应用所有快速修复**来完成。”

> **提示或重要说明**
>
> 如此显示。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果你对本书有任何疑问，请发送电子邮件至`customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误：** 尽管我们已竭尽全力确保内容的准确性，但错误仍会发生。如果你在本书中发现错误，请向我们报告，我们将不胜感激。请访问`www.packtpub.com/support/errata`并填写表单。

**盗版：** 如果你在互联网上以任何形式发现我们作品的非法副本，请提供相关地址或网站名称，我们将不胜感激。请通过`copyright@packt.com`联系我们，并提供相关材料的链接。

**如果你有兴趣成为作者：** 如果你有专长的主题，并且有兴趣撰写或合著一本书，请访问`authors.packtpub.com`。

## 分享你的想法

读完*云原生开发与Jakarta EE迁移*后，我们很乐意听到你的想法！请点击此处直接前往本书的亚马逊评论页面分享你的反馈。

你的评论对我们和技术社区都很重要，并将帮助我们确保提供卓越品质的内容。
