# UiPath Associate 认证指南：Associate 认证考试通过指南（模拟测试与测验）





献给我的父亲——您肩头的引导之手将永远与我同在。
– Niyaz Ahmed

我想感谢我的父亲 Sesha、我的妻子 Mihiri 和我的女儿 Emmie，在人生的所有艰难时刻都扶着我的手。同时，特别感谢我两位出色的导师 Erik Gillet 和 Lucas Pimenta，感谢他们始终激励和指导我。最后，我想感谢整个 UiPath 社区、UiPath 团队、我在 Boundaryless Group 的团队以及所有朋友，感谢他们提供的所有机会，并在许多方面帮助我改变人生。
– Lahiru Fernando

献给任何想要通过 UiPath Associate 认证、开始学习 UiPath、转向 UiPath 职业生涯，或仅仅想了解这项技术的人——我相信本书无疑将帮助您实现 UiPath 目标。感谢合著者、评审员和 Packt Publishing 团队的出色协作，他们以最简化的方式创建本书内容，帮助任何人学习 UiPath 并获得 UiPath Associate 认证。

献给我的妻子 Anusree、儿子 Saijesh 和女儿 Dakshya，感谢他们允许我体验撰写本书的过程。
– Rajaneesh Balakrishnan

## 贡献者

## 关于作者

**Niyaz Ahmed** 是 UiPath 的技术项目经理，致力于使当前和未来的劳动力为自动化就绪的世界做好准备。他通过与领先的机构（包括大学、学院和高中）合作，管理 UiPath Academic Alliance 和 Learning Partner 计划。他还是 Udacity RPA 开发者纳米学位课程的讲师。此外，他通过指导学习者开发专注于开发和部署软件机器人的专业级技能，继续传播 RPA。

> *特别感谢 Alok Shrivastava、Rajesh Nambiar、Leah Hojem 以及所有在本书不同阶段帮助过我的 UiPath 同事。*

**Lahiru Fernando** 是 UiPath MVP、RPA 执行团队负责人、UiPath 社区领袖、导师、讲师和公众演讲者，在 IT 行业拥有 11 年以上的经验。他于 2011 年在 Staffordshire University 完成专业软件工程学位后开始了自己的职业生涯。他曾在银行、金融、市场研究和农业领域参与多个商业智能项目。2019 年转入 RPA 领域后，他作为首席 RPA 解决方案架构师，在金融、零售、银行、儿童福利、石油和天然气、电信及服装领域主导了多个成功的 RPA 项目。他还负责 Boundaryless Automation 的 RPA 赋能和培训工作。

Rajaneesh Balakrishnan 是 2AutomateAnything 的创始人，自 2020 年以来一直是 UiPath 高级认证 RPA 开发者和 UiPath MVP。他最初从事质量分析师工作，后来成为软件测试实践负责人，随后成为拥有大量自动化工具实施经验和多个领域经验（包括 BFSI）的 RPA 负责人。他在 IT 行业拥有 15 年以上的经验，已为许多知名客户成功实施了 UiPath RPA，为组织节省了大量时间和成本。他也是一位充满热情的教师，举办各种企业和个性化培训活动，指导许多人转向 UiPath 的 RPA 职业生涯。他曾在 YouTube、Udemy 和 LinkedIn 上分享教育内容，以支持 UiPath 社区。

## 关于审稿人

在获得计算机科学硕士学位后，**Mukesh Kala** 开始了软件开发员的职业生涯。他拥有 8 年以上的专业经验，目前担任高级 RPA 分析师，负责银行和金融服务行业的自动化工作。

作为学习和知识共享力量的坚定信徒，Mukesh 定期参加 UiPath 社区活动，培训并帮助技术初学者。Mukesh 于 2021 年被评为 UiPath MVP，并运营着 YouTube 频道 _Mukesh Kala 的教程_。在闲暇时间，他喜欢阅读书籍和玩魔方。

**Andy Menon** 是 2021 年的 UiPath MVP。在日常工作中，他是一名集成工程师，拥有为企业集成系统的经验。

Andy 也是 RPA Vanguard 的创始人，以及同名 YouTube 频道的创办人，该频道通过真实案例教授 RPA。他在 Udemy 上的企业 RPA 课程在 CourseMarks 近 100,000 门课程中排名前 20%。

在闲暇时间，Andy 喜欢钻研业余电子、3D 打印和 **物联网（IoT）**，尝试使用 Arduino、Raspberry Pi 和 ESP8266 系列微控制器。

Andy 也是 _赋予自己 IoT 超能力_ 一书的 Kindle 出版作者。

他已在 Instructables 和 Thingiverse 上在线发布了几个业余项目和设计。

## 前言

UiPath 是机器人流程自动化（RPA）行业中最受欢迎的厂商。如果您是渴望在该行业取得成功的 RPA 爱好者或公民开发者，获得此认证可以帮助您获得认可，并使用 UiPath 应对现实世界的挑战。

《UiPath Associate Certification Guide》（UiPath 助理认证指南）全面、最新地覆盖了 UiPath RPA 助理认证考试的内容，帮助您一次通过并获得认证。本书以清晰、简洁的方式编写，每章末尾均附有自评问题、带答案的测验、考试技巧以及附有详细答案和解析的模拟考试。您将首先掌握 UiPath RPA 的基本概念，然后深入探讨助理认证所需的所有概念。最后，您将通过获取所需知识并使用示例业务案例来实施这些技能，从而培养 UiPath 技能。

阅读完本 UiPath 书籍后，您将掌握通过考试所需的一切，获得处理现实世界案例研究所需的知识，并学习如何应用各种概念来构建企业级用例。

## 本书适用人群

本 RPA 书籍适用于技术及半技术岗位人员，例如公民 RPA 开发者、初级 RPA 开发者、RPA 开发者、解决方案架构师、业务分析师、系统管理员、大学生和毕业生，以及 UiPath 和合作伙伴在售前、服务、支持等方面的员工。如果您希望获得认证并满足最大 RPA 厂商的行业标准，那么本书适合您。




## 本书涵盖的内容

第 1 章，自动化之旅与识别适合的业务流程，探讨了数字化转型的重要性及其驱动因素。本章还展示了业务中的一些 RPA 用例，随后介绍了识别合适自动化流程所需的关键步骤。在开始 RPA 之旅之前，了解这些要点是必要的，因为本章涵盖的主题能让你了解全球流程自动化的现状、其重要性以及涉及的关键资源。

第 2 章，UiPath 生态系统，概述了使用 UiPath Studio 构建简单到复杂自动化所需的 UiPath 企业级 RPA 平台的每个组件。本章还包含关于安装 UiPath 和设置 UiPath Studio 配置文件的信息。

第 3 章，UiPath Orchestrator 简介，重点介绍 UiPath 云平台和 UiPath Orchestrator。UiPath Orchestrator 在自动化中扮演着重要角色，因为它充当所有已部署机器人和流程的中央控制单元。因此，了解 Orchestrator 的使用和概念以管理你的虚拟劳动力至关重要。

第 4 章，在 UiPath 生态系统中创建、部署和执行 RPA 流程，提供了创建机器人并在机器人与 UiPath Orchestrator 之间建立连接的分步说明。示例自动化项目在 UiPath Studio 中创建，然后发布到 Orchestrator，并通过 UiPath Assistant 和有人值守自动化由机器人执行。

第 5 章，了解变量和参数，探讨变量和参数——任何编程语言的两个关键组件。本章介绍了 UiPath Studio 如何使用变量和参数，以及最常用的数据类型。

第 6 章，了解不同的控制流，涵盖控制流，它使机器人在评估任何业务条件时能够做出关键决策。各种控制流活动（如序列、流程图和循环）通过实时示例进行探讨，以便更好地理解。

第 7 章，使用 UiPath 处理数据，关注数据——每个组织中最关键的资源。我们需要修改、构建、格式化和排序数据，将其转化为有意义的信息。本章介绍了基于各种数据类型处理数据的不同方法。

第 8 章，探索 UiPath 选择器，探讨自动化中一个主要的重要主题——选择器。各种选择器类型（如完整选择器、部分选择器、静态选择器和动态选择器）均有示例进行讨论。某些选择器工具（如 UI Explorer 和 Selector Editor）也被有效使用，以充分利用选择器功能。

第 9 章，了解数据表的用途并探索 Excel 自动化，聚焦于 Microsoft Excel——几乎每天都用于执行不同功能的工具。我们在 Excel 中处理的数据更具结构化表格形式。在本章中，我们重点介绍如何使用 UiPath 自动化 Excel，以及如何使用数据表变量来支持 Excel 自动化。

第 10 章，探索用户界面和用户/机器人交互，探讨用户界面的自动化。本章讨论了窗口、文本字段、按钮、下拉列表、复选框、文本框等 UI 元素的自动化。为实现有效的自动化，探讨了各种输入操作和输出操作。图像自动化和文本自动化等一些高级 UI 自动化也有讨论。

第 11 章，自动化 PDF 数据提取，涵盖 PDF 文件——广泛用于组织之间共享信息。本章重点介绍如何使用 UiPath 自动化 PDF 文件，以及可用于构建成功 PDF 自动化的活动有哪些。

第 12 章，探索 UiPath 电子邮件自动化，关注电子邮件——个人和组织最常用的数字通信形式之一。本章探讨如何自动发送电子邮件、接收电子邮件、回复电子邮件、附加和下载文档附件，以及为电子邮件使用定义的消息模板。通过实时示例探讨了各种电子邮件活动和电子邮件功能，以便更清晰地理解。

第 13 章，调试，向你展示如何调试一个流程，即从给定项目中识别并删除错误，使其正确运行。

第 14 章，发票处理 – 分发器，向你展示如何设计发票处理的分发器模型，从发票中提取相关详细信息并将其上传到 Orchestrator 队列。

第 15 章，发票处理 – 执行器，向你展示如何设计发票处理的执行器模型，即从 Orchestrator 队列中检索事务项目并将发票数据输入到 CRM 中。

第 16 章，如何准备以及预期内容，帮助你更多地了解考试和认证凭证。

第 17 章，模拟考试 1，是一份旨在帮助你准备 UiPath RPA Associate v1.0 考试的练习测试。练习测试测试你的知识，并帮助你了解实际考试的情况。

第 18 章，模拟考试 2，是一份旨在帮助你准备 UiPath RPA Associate v1.0 考试的练习测试。练习测试测试你的知识，并帮助你了解实际考试的情况。

## 如何充分利用本书

在本书中，我们使用 UiPath Studio、UiPath Orchestrator 和 UiPath Assistant，因此请确保你已安装正确的版本。

你还需要访问几个应用程序，如各章节 *技术要求* 部分中所述。

| 本书中涵盖的软件/硬件 | 操作系统要求 |
| --- | --- |
| UiPath Studio 202X.X | Windows 8.1+ |
| UiPath Orchestrator 202X.X | |
| UiPath Assistant 202X.X | |

本书与出版时 UiPath Studio、Orchestrator 和 Assistant 的最新版本兼容（UiPath 202X.X）。

整个 UiPath Associate 认证均基于 UiPath Studio 的经典设计体验。现代设计体验不是认证的一部分；因此，本书也基于经典设计体验。

**如果你使用的是本书的数字版本，我们建议你亲自输入代码或从本书的 GitHub 仓库获取代码（下一节中提供了链接）。这样做可以帮助你避免与复制和粘贴代码相关的任何潜在错误。**

## 下载示例代码文件

你可以从 GitHub 上的 https://github.com/PacktPublishing/UiPath-Associate-Certification-Guide 下载本书的示例代码文件。如果代码有更新，将在 GitHub 仓库中更新。

我们还在 https://github.com/PacktPublishing/ 提供丰富图书和视频目录中的其他代码包。请查看！

## 下载彩色图片

我们还提供一个包含本书中使用的截图和图表彩色图片的 PDF 文件。你可以在此处下载：https://static.packt-cdn.com/downloads/9781801072731_ColorImages.pdf。

## 本书中使用的约定

本书中使用了一些文本约定。

文本中的代码：表示文本中的代码字词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如：“Create the EmailAddress, Password, FirstName, LastName, and TermsAndConditions variables of the String type, and have the default values of EmailAddress, FirstName, and LastName。”

代码块按如下形式设置：

```
"Hello " + In_UserName + ", It's nice to see you. This is my first attempt to join two workflows."
```

**粗体：** 表示新术语、重要词汇或屏幕上看到的词汇。例如，菜单或对话框中的词汇以 **粗体** 显示。示例如下：“单击活动上的 **数据表...** 按钮打开 **构建数据表** 屏幕。此屏幕可用于添加列、行和更改列属性。”

提示或重要说明
显示如下。

## 联系我们

始终欢迎读者反馈。

**一般反馈：** 如果你对本书的任何方面有疑问，请发送电子邮件至 customercare@packtpub.com，并在消息主题中注明书名。

**勘误：** 虽然我们已尽一切努力确保内容的准确性，但错误仍难免发生。如果你在本书中发现错误，我们将不胜感激如果你能将其报告给我们。请访问 www.packtpub.com/support/errata 并填写表格。



**盗版：**如果您在互联网上以任何形式发现我们作品的非法复制件，若能向我们提供位置地址或网站名称，我们将不胜感激。请通过 copyright@packt.com 联系我们，并附上材料链接。

**如果您有兴趣成为作者：**如果您在某个领域有专长，并有兴趣编写或参与编写图书，请访问 authors.packtpub.com。

## 分享您的想法

阅读完《UiPath Associate Certification Guide》后，我们非常乐意听取您的想法！请点击此处直接前往本书的 Amazon 评论页面，分享您的反馈。

您的评价对我们和技术社区至关重要，将帮助我们确保提供高质量的内容。
