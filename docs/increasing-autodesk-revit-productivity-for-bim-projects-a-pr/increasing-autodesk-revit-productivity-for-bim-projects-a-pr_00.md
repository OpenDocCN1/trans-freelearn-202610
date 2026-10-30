# 提升 BIM 项目的 Autodesk Revit 生产力：使用 Revit 工作流提高 BIM 项目生产力和效率的实用指南

Fabio Roberti:

To my wife Lucie and my daughters Evelyn and Hellen for their unbounded support and love while I was writing the book. Thank you to my parents for the love and education. I also would like to thank Packt Publishing for the opportunity to write this book.

Decio Ferreira:

First, I would like to dedicate this book to my wife Maria Joao Pinto and to my big "A" (Andre) and SMALL "a" (Afonso) for their support, guidance, inspiration and love – this is for YOU. No less important are all my friends, colleagues, students, readers, and followers for the continuous questions, feedback, ideas, problems, and shares that helped shaping my career. A special THANK YOU to my parents for everything that I am. And I would like to express my gratitude to the entire team at Packt Publishing for this opportunity and for their support and collaboration.

## Contributors

### About the authors

![](img/1961add582dd4a589b2eac3dda140e38_4_0.png)

**Fabio Roberti**

**Fabio Roberti** is a Regional Practice Technology Leader (EMEA) and Associate at HKS Architects, where he develops and implements the digital strategy to consolidate processes and workflows to enhance project deliverables.

He supports BIM processes in innovative ways to improve technology adoption with a thorough understanding of British Standards and the ISO 19650 framework and has many years of experience using Revit across all design stages.

He holds a Master's degree in Architecture, the Autodesk Revit Architecture Professional Certificate, and a BIM Level 2 Certificate from BRE Academy. Fabio has presented at multiple conferences, including Digital Construction Week and Autodesk University in London and Las Vegas.

His passions for digital technology and teaching others new skills helped him achieve his company's objectives and support many colleagues over the years. Besides BIM excellence, Fabio is interested in the rapidly advancing artificial intelligence sector, programming, and technologies that support superior construction deliverables.

LinkedIn Profile: linkedin.com/in/froberti

Twitter: @F_Roberti_

![](img/1961add582dd4a589b2eac3dda140e38_5_0.png)

**Decio Ferreira**

**Decio Ferreira**, with over 20 years of professional experience as an Architect, is a BIM and Design Systems BIM Coordinator at Foster+Partners, where he is responsible alongside with his team for developing and implementing standards, methods, and procedures across the practice. He is a strong, committed individual who thrives when developing and researching BIM strategies, workflows, and approaches to improve what he believes to be the most important: collaboration.

He holds three BIM certifications: BIM Level 2 Fundamentals and BIM Level 2 BS1192-2 Project Information Manager and Task Information Management, both from BRE Academy, and BS EN ISO 19650-3:2020 from BSI, both in London. In terms of software, he has worked with Revit since 2000 and holds Autodesk Revit Architectural, Structural and Mechanical Professional Certificates (since version 2011), along with an AutoCAD Professional Certificate (since version 2005), and is an Autodesk Certified trainer.

He lectures at several academic institutions as a guest lecturer and actively lectures at NossoBIM on the International Master BIM Manager program and also at Polytechnic Institute ISEP (Instituto Superior de Engenharia do Porto), providing a high level of expertise in the BIM software training field.

Decio has presented at multiple conferences, including at Autodesk University in Las Vegas and Sao Paulo. Decio is passionate about photography, planes, and traveling, which allows him to contact, learn about, and understand new cultures and different lifestyles. For him, there is nothing more important than his family!

LinkedIn Profile: `linkedin.com/in/deciosantosferreira`

Twitter: `@decioferreira`

### About the reviewer

With a strong focus on quality, **Nuno Araujo Nogueira** – a Portuguese architect, born in 1978 – has a keen interest in the science of building technology, including standards, workflows, and BIM strategies. A BIM manager with over 15 years' experience managing projects in Portugal, Kuwait, and Egypt ranging up to $1Bn in value, and leading multicultural teams locally and remotely.

Although his main working tool is currently Navisworks, he has been using Revit since 2007, which he uses exclusively to develop complete projects.

Currently, he is the PMC BIM manager at the largest archaeological museum in the world – the Grand Egyptian Museum. Previously, he was the Main Contractor BIM Manager of an iconic supertall building in Kuwait – the National Bank of Kuwait Headquarters.

## 前言

Revit 使建筑师、设计师、工程师、建模人员、**建筑信息模型 (BIM)** 协调员和 BIM 经理能够创建 BIM 模型并分析智能数据，以改进设计和施工。BIM 推动了工程和建筑行业的转型，在传统 2D CAD 流程的基础上提供了诸多优势，其方法论以信息为核心，提高了生产力。本书采用实践导向的方法来有效实施这一新方法论。

本书以分步解释基本概念和实践例子的方式，首先讲解了 Revit 中的生产力原则和 BIM 项目的数据管理。随后，您将了解启动 BIM 项目所需的主要 BIM 文档，包括合同、**信息交换要求 (EIR)** 和 **BIM 执行计划 (BEP/BXP)**。之后，您将学习如何创建 Revit 模板、启动 Revit 项目，并探索 Revit 的核心功能以提高生产力。在打好基础后，您将学习 Revit 插件，并使用 Dynamo 进行可视化编程，使用 Power BI 分析 BIM 信息。

通过阅读本书，您将深入了解如何提高 Revit 中的生产力，并能够在项目中应用多种工作流来高效管理 BIM。

## 本书适合谁阅读

本书适合有兴趣学习 Revit 最佳实践的建筑师、设计师、工程师、建模人员、BIM 协调员和 BIM 经理。提高 BIM 项目中 Autodesk Revit 的生产力，将帮助您探索一种将信息管理和研究相结合的方法，以在 Revit 工作时获得高质量输入。

本书也面向希望在 Revit 中优化日常工作、用更短时间产出更多成果的人们。这种实践性的效率提升方法配有实用的讲解，将让您的生产力实现阶跃式提升。

## 本书涵盖内容

第 1 章“Revit 生产力简介”解释了如何结合信息管理原则和最佳 Revit 实践来提高生产力。本章还提供了 Revit 的推荐设置和 BIM 实施概述。

第 2 章“项目数据管理”介绍了启动 BIM 项目时的关键数据，并概述了委任、BIM 文档、项目功能、资产管理以及制定启动 BIM 项目的行动计划。

第 3 章“Revit 模板和标准”解释了如何创建 Revit 模板和管理图形标准。

第 4 章“高效启动 Revit 项目”概述了 Revit 坐标系，并解释了如何启动 Revit 项目和设置项目坐标。

第 5 章“Revit 核心功能”解释了如何通过明细表管理、使用关键注释进行规格说明、设计选项、Revit 阶段、范围框、工作共享和出图最佳实践来提高 Revit 中的生产力。

第 6 章“使用 Dynamo 进行可视化编程”提供了 Dynamo 的基础知识，并展示了掌握该应用程序以提高 Revit 效率的重要性。

第 7 章“使用 Power BI 进行仪表板分析”解释了从 Revit 导出数据并导入到 Power BI 以及使用 Power BI 分析数据的过程。

第 8 章“与顾问协作”探讨了与顾问协作的策略，并提供了有关增强 Revit 协作的 Revit 工具信息，例如复制/监视、协调检查和共享视图。

第 9 章“使用插件提高生产力”教授如何管理 Revit 插件，并发现最常用的 Revit 插件以提高项目中的生产力。

第 10 章“提升性能”探讨了提高 Revit 性能的其他信息，包括建模最佳实践、内容管理、优化 Revit 性能和 BIM 360 最佳实践。

## 如何充分利用本书

本书大部分章节要求您使用 Revit 2021 或更高版本。本书中的信息适用于任何未来的 Revit 版本，因为其中解释的许多生产力原则并不基于特定 Revit 版本。

某些章节还要求您使用以下软件：AutoCAD、Dynamo BIM、Microsoft Excel 和 Microsoft Power BI。

各章按特定的生产力提升领域划分，您可以根据需要阅读本书。一旦开始阅读某一章，您会发现其组织方式便于您理解和实践所提供的内容。

本书旨在通过实用的讲解为您提供提升效率的实践方法，这将使您的生产力实现阶跃式提升。

阅读本书不需要任何先备知识或技能。不过，对 Revit 有基本了解会有所帮助。

## 下载示例文件

您可以从 GitHub 下载本书的示例文件：`https://github.com/PacktPublishing/-Increasing-Autodesk-Revit-Productivity-for-BIM-Projects`。如果有更新，会放在现有的 GitHub 仓库中。

我们还有其他来自丰富书籍和视频目录的代码包，可在 `https://github.com/PacktPublishing/` 获取。去看看吧！

## 下载彩色图片

我们还提供一份包含本书截图/图表彩色图片的 PDF 文件。您可以在此下载：`https://static.packt-cdn.com/downloads/9781800566804_ColorImages.pdf`。

## 使用的约定

本书通篇使用了许多文本约定。

**文本中的代码：** 表示文本中的代码词、数据库表名、文件夹名称、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如：“输入参数名称 `Area Percentage`。”

**粗体：** 表示新术语、重要单词或屏幕上显示的单词。例如，菜单或对话框中的文字会以这种形式出现在文本中。例如：“在 **排序/分组** 选项卡中，选择 **排序方式：标高**。”

> **提示或重要说明**
> 
> 以此形式显示。



在阅读本书时，您会看到二维码，方便您访问相关网站。您可以使用移动设备的相机扫描二维码，打开网站链接或查看图片。

二维码示例如下：

![](img/1961add582dd4a589b2eac3dda140e38_16_0.png)

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果您对本书的任何方面有疑问，请在邮件主题中注明书名，并发送邮件至 customercare@packtpub.com。

**勘误：** 尽管我们已竭尽全力确保内容的准确性，但错误仍可能发生。如果您在本书中发现错误，我们将不胜感激，请访问 www.packtpub.com/support/errata，选择您的书籍，点击“勘误提交表”链接，并填写详细信息。

**盗版：** 如果您在网上发现我们作品以任何形式出现的非法副本，我们将不胜感激，请提供位置地址或网站名称。请通过 copyright@packt.com 联系我们，并附上材料链接。

**如果您有兴趣成为作者：** 如果您在某方面有专长，并且有兴趣撰写或参与编写一本书，请访问 authors.packtpub.com。

## 评论

请留下评论。在您阅读并使用本书后，何不在您购买本书的网站上留下评论呢？潜在读者可以看到并使用您的公正意见来做出购买决定，我们 Packt 可以了解您对我们产品的看法，我们的作者也能看到您对他们图书的反馈。谢谢！

有关 Packt 的更多信息，请访问 packt.com。
