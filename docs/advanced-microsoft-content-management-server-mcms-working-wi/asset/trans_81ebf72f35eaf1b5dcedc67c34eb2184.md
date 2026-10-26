

- 在演示模式下显示 XML 279
- 使用 DataGrid 占位符控件 281
- 关闭离开警告提示 282
- 摘要 284

## 第 9 章：验证占位符控件 285

- ASP.NET 验证控件的局限性 285
- MCMSValidators 项目 286
- 验证 HtmlPlaceholderControl 287
- 获取 HtmlPlaceholderControl 的当前值 287
- 检查 HtmlPlaceholderControl 是否为空 289
- 使用正则表达式匹配标签 291
- 构建必需的 HTML 占位符验证器 292
- 重写 ControlPropertiesValid() 方法 293
- 重写 OnPreRender() 方法 293
- 重写 AddAttributesToRender() 方法 295
- 重写 EvaluateIsValid() 方法 297
- 将自定义验证器添加到模板文件 297
- 防止包含无效内容的页面被保存 298
- 保存新页面并进行验证 299
- 保存并退出时进行验证 299
- 保存时进行验证 300
- 将修改后的按钮添加到作者控制台 301
- 实现服务器端验证 302
- 带验证的 HtmlPlaceholderControl 303
- 带服务器端验证的 RequiredHTMLPHValidator 305
- 检查输入文本的长度 306
- 取消保存操作 308
- 验证 SingleImagePlaceholderControl 309
- 获取 SingleImagePlaceholderControl 的值 309
- 检查 SingleImagePlaceholderControl 是否为空 310
- RequiredImagePHValidator 310
- 验证 SingleAttachmentPlaceholderControl 312
- SingleAttachmentPlaceholderControl 的存储值 313
- 检查 SingleAttachmentPlaceholderControl 是否为空 313
- RequiredAttachmentPHValidator 314
- 摘要 316

## 第 10 章：静态页面分发 317

### 简述 Site Stager 318

- 安装 Site Stager 319
- 定义目标用户 319
- Site Stager 管理对话框 320
- 创建分发配置文件 321
- 运行作业 323
- 安排配置文件 324
- 删除配置文件 324

### ASP.NET Stager 应用程序 325

- DotNetSiteStager 项目 325
- 配置 Stager 设置 326
- 将消息记录到日志文件 329
- 以 'Stage As' 用户身份登录 330
- 显示隐藏的发布项 331

### 分发频道和发布项 331

- 获取起始频道 331
- 遍历频道树 332
- 发出 HTTP 请求 333
- 获取响应并创建文件 334

### 分发附件 336

## 第 11 章：使用 MCMS Web Services 的 InfoPath 347

### 项目概述 347

### 什么是 ASP.NET Web Services？ 349

### 创建 MCMS Web Service 项目 349

### 创建业务层 350

- 维护自定义属性 351
- 创建轻量级模板类 354

## 引言

继 *Building Websites with Microsoft Content Management Server*（Packt Publishing，2005 年 1 月，ISBN 1-904811-16-7）之后，本书将 MCMS 开发提升到更高水平，无论是功能还是集成度。像其前作一样，本书包含大量代码示例和从未公开过的 MCMS 秘诀。

Microsoft Content Management Server 2002 是一个动态 Web 发布系统，可帮助您快速且经济地构建网站。MCMS 提供管理、创作和数据管理功能，而您提供网站界面、逻辑和工作流。本书中还介绍了 Microsoft SharePoint Portal Server（SPS）。SPS 2003 使企业能够部署智能门户，无缝连接用户、团队和知识，以便人们能够利用跨业务流程的相关信息，从而更高效地工作。

您已经掌握了 MCMS 的基础知识，并搭建了自己的 MCMS 安装环境。但您只触及了表面。本书是您充分利用 MCMS 和 SPS 投资的门户，也是让这两个应用程序协同工作以提供卓越内容交付和易于维护性的途径。

### 本书涵盖内容

*第 1 章* 通过构建 **CMS Explorer** 管理工具来管理 MCMS 网站，展示了 MCMS 发布 API（PAPI）的强大功能。*第 2 章* 在 CMS Explorer 的基础上增加了管理频道和发布项的能力。*第 3 章* 通过 PAPI 查看模板、模板库和资源的创建、提交、复制、移动和删除。

在 *第 4 章* 中，您将学习如何为搜索索引准备发布项。我们探讨了几种可以提高搜索结果准确性并优化搜索引擎的技巧。

SharePoint Portal 技术通过提供协作、文档库和搜索功能来补充 MCMS，从而增强 MCMS 强大的发布工作流。*第 5 章* 指导您使用 SharePoint Portal Server 为 MCMS 站点添加搜索功能，无论是通过 MCMS Connector for SharePoint Technologies 还是自行构建解决方案。

*第 6 章* 演示了如何使用 MCMS Connector for SharePoint 构建自己的组件，以在 MCMS 和 SharePoint 之间共享内容。*第 7 章* 展示了如何构建 Web Parts，将 MCMS 内容集成到 SharePoint 门户站点中。

*第 8 章* 讨论了五个自定义占位符控件，它们提供了默认控件中没有的、经常被请求的特性：日期时间选择器占位符控件、允许多个附件的占位符控件、图像轮播占位符控件、用于存储各种 HTML 标签的占位符控件，以及 DataGrid 占位符控件。

内容验证是许多 MCMS 实现中的关键需求。第 9 章探讨了如何将 ASP.NET 验证技术应用于每个开箱即用的占位符控件。

静态页面通常用于直接邮件、帮助文件，甚至用于归档目的。第 10 章讨论了可用于创建发布项静态快照的几种技术。

创作体验并不总是需要通过浏览器进行。第 11 章详细介绍了维护内容的一种对作者友好的方式。在本章中，我们利用 InfoPath 的强大功能，快速创建 GUI，允许作者直接从 Microsoft Word 提交内容，并借助 MCMS Web Services。

自 2002 年 MCMS 发布以来，许多技术已经发生了变化。使用 RSS 进行网站聚合已成为常态，为此，第 12 章指导您完成创建网站最近更改的动态 RSS Feed 的步骤。

最后，第 13 章提供了大量关于 MCMS 的内部技巧和窍门，以及常见 MCMS 问题的解决方案，包括如何将发布项恢复到以前版本、更改发布项的模板、构建回收站，以及使用站点部署 API 导出资源库项目等宝贵技巧。

### 阅读本书所需条件

本书面向具备扎实 C# 基础的 ASP.NET 开发人员。要使用本书，您需要能够访问以下资源：

- Visual Studio .NET Professional 或更高版本（2002 或 2003 版本）。
- Microsoft Content Management Server 2002 Developer 或 Enterprise 版。您也可以使用 Standard 版，但请注意某些功能将不可用。Enterprise 版的 120 天评估版可在 http://www.microsoft.com/cmserver 获取。

要安装和运行 Microsoft Content Management Server 2002，您需要以下条件：

- .NET Framework 1.0 或 1.1。
- 以下操作系统之一：Windows Server 2003、Windows 2000 Professional、Windows 2000 Server 或 Windows XP Professional。
- SQL Server 2000 Developer 或 Enterprise 版的安装。

部分章节使用了 Microsoft SharePoint Portal Server 2003。对于这些章节，您需要：

- .NET Framework 1.1。
- Windows Server 2003。
- SQL Server 2000 Developer 或 Enterprise 版的安装。
- SharePoint Portal Server 2003 的安装。120 天评估版可在 http://www.microsoft.com/sharepoint 获取。

