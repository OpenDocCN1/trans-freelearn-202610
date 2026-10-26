
这些只是你在设计和开发自己的实现时需要考虑的部分额外问题。从 SharePoint 列表服务 Web 服务中拉取内容的难点，已通过这个简短的占位符控件得到了演示。你可以发挥想象力，根据自己的特定需求来定制这个占位符控件。

## 摘要

在本章中，我们介绍了在 SharePoint 站点中发布 MCMS 内容，以及在 MCMS 站点中发布 SharePoint 内容的多种方式。在这两种情况下，我们都探讨了 Microsoft 提供的免费下载——用于 SharePoint Technologies 的 MCMS 连接器——在两种环境之间发布内容时能提供哪些协助。

为了在 MCMS 站点中发布 SharePoint 内容，我们在 MCMS 模板上使用了 SharePoint 文档占位符控件，以显示存储在 SharePoint 文档库中的文档的属性和/或内容。此外，我们还构建了一个新的占位符控件，作为 SharePoint 列表内容的聚合器。

为了在 SharePoint 站点中发布 MCMS 内容，我们使用了连接器附带的页面列表 Web 部件，将 MCMS 站点中指定频道内的所有发布项以列表形式显示在 SharePoint 中。我们还介绍了连接器附带的两个管理 Web 部件——生产中的 MCMS 页面和等待审批的 MCMS 页面，以及它们如何协助 MCMS 站点的发布工作流。

## 构建 SharePoint Web 部件

在前面的章节中，我们通过自定义代码以及使用用于 SharePoint Technologies 的 MCMS 连接器随附的组件，了解了集成 MCMS 和 SharePoint 的几种不同方式。我们尚未涉及的一个重要方面，是为 SharePoint 创建自定义 Web 部件，以便将 MCMS 内容集成到 SharePoint 站点或门户中。在本章中，我们将创建一个可配置的 Web 部件，作为指定频道中发布项的导航组件。但在深入探讨我们要构建的 MCMS 导航 Web 部件之前，先让我们了解一下什么是 Web 部件。

Microsoft SharePoint Portal Server (SPS) 和 Windows SharePoint Services (WSS) 在称为 **Web 部件页** 的页面中显示内容和交互式组件。构成这些页面的构建块是 **Web 部件区域**，可以包含一个或多个 Web 部件。具有所需权限的业务用户可以根据需要将这些 Web 部件排列在 Web 部件页上。

![](img/442e26bf28f2df81fbf8dff3478f88e3_215_0.png)

Web 部件是一种单一模块，可以排列在 Web 部件页上以提供特定功能，例如日历。MCMS 仅随附三个占位符控件，而 SPS 和 WSS 随附了许多已安装的 Web 部件，用于处理公告、联系人、任务、事件、数据查看和 XML。这些 Web 部件提供了一个良好的起点，并且易于配置，无需开发人员干预即可满足大多数用户的需求。

## 构建 SharePoint Web 部件

由于多种原因，包括更紧密的集成、更具体的需求以及性能或复杂性方面的问题，您可能需要从头创建 Web 部件。在设计 Web 部件的早期阶段，开发人员可以有多种途径：

- 配置开箱即用的 WSS/SPS 列表和 Web 部件，以提供所需的数据存储、数据输入和呈现方法
- 从零开始将 Web 部件开发为 ASP.NET 服务器控件
- 将业务逻辑开发为 Web 服务，并使用 XML Web 部件或 DataView Web 部件作为图形用户界面
- 开发单独的 Web 窗体，并使用 IFrame 集成门户和卫星应用程序。使用此方法时应考虑浏览器兼容性
- 将 Web 部件设计和开发为 .NET 用户控件，并利用通用 Web 部件（如 'SmartPart' (http://www.smartpart.info)）来承载你的 .NET 用户控件

每种方法都有优点和缺点，应在设计时进行权衡，例如开发难易度、可扩展性、可伸缩性、互操作性、部署和安全性。下表总结了这些内容：

| 方法 | 现有 Web 部件 | 使用 Web 服务作为业务层，搭配 XML Web 部件 / DataView Web 部件 | 与 SmartPart 集成的用户控件 | ASP.NET 服务器控件 | IFrame 到 ASP.NET Web 窗体 |
|---|---|---|---|---|---|
| 开发难易度 | 无需任何开发 | 简单 | 需要了解 SmartPart，但本质上就是 ASP.NET 用户控件开发 | 复杂 | 简单 |
| 可扩展性 | 取决于特定 Web 部件提供的功能 | 开发人员对 UI 的控制能力有限，但获得了 FrontPage 2003 作为开发工具，以及独立的 ASP.NET 业务层。 | 可扩展性极强 | 可扩展性极强 | 可扩展性极强 |
| 可伸缩性 | 随 WSS/SPS 服务器扩展 | 端点 Web 服务必须按需扩展。 | 随 WSS/SPS 服务器扩展 | 随 WSS/SPS 服务器扩展 | 通过 IFrame 内嵌的 ASP.NET 应用程序必须按需扩展 |
| 互操作性 | 可以轻松与 WSS/SPS 的其他部分交互 | 与 WSS/SPS 对象交互更为复杂，但仍然可行。 | 可以轻松与 WSS/SPS 的其他部分交互 | 可以轻松与 WSS/SPS 的其他部分交互 | 与 WSS/SPS 对象交互更为复杂，但仍然可行。 |
| 部署 | 开箱即用的 Web 部件无需部署，可下载的 Web 部件部署也非常简单 | 无需部署 | 需要安装 SmartPart Web 部件。 | 复杂 | 简单 |
| 安全性 | 无安全影响 | 对 SPS/WSS 无安全影响，因为安全将由 ASP.NET Web 服务强制实施 | 无安全影响 | 安全由 SPS/WSS 强制实施。 | 由 ASP.NET Web 应用程序强制实施 |
| 备注 | 需要的开发知识最少 | 非常适合需要在 Web 部件页中显示 WSS/SPS 外部数据的 .NET 开发人员 | 允许你将 .NET 用户控件作为 Web 部件运行，并允许在 Web 部件框架内开发丰富的用户界面。 | 开发时提供了最大的灵活性，但需要手动定义 UI。在 Web 部件框架内执行。 | 允许通过 SharePoint 使用单独的 ASP.NET Web 应用程序。 |

在本章中，我们将重点介绍 ASP.NET 服务器控件方法。

## SharePoint MCMS 导航控件

在本章中，我们将开发一个 Web 部件作为服务器控件，具体做法是构建一个用作 MCMS 导航组件的 Web 部件。我们之所以选择服务器控件方法，是因为它是最灵活的方法。具有 WSS/SPS Web 设计器权限的最终用户将能够把 Web 部件添加到 Web 部件页，并通过指定 MCMS 频道来配置该 Web 部件。所配置的 MCMS 频道中的项目将使用 XSLT（可扩展样式表语言转换）样式表在 Web 部件中呈现，以生成如下显示：

![](img/442e26bf28f2df81fbf8dff3478f88e3_218_0.png)

要构建此 Web 部件，我们将执行以下步骤：

- 准备开发环境
- 创建 Web 部件项目
- 创建 Web 部件
- 定义 Web 部件的自定义属性
- 开发 Web 部件的自定义属性
- 构建数据层
- 构建表示层
- 部署 Web 部件

> 我们将要构建的 Web 部件既可在 SPS 也可在 WSS 下运行，因为我们不会使用 Microsoft SharePoint Portal Server 的任何特定功能。

## 准备开发环境

为了最有效地将 Web 部件开发为 ASP.NET 服务器控件，我们需要安装 Web 部件所需的 Visual Studio .NET 模板。这些模板不随 Visual Studio .NET 2003 提供，但可从 Microsoft 网站下载：

http://www.microsoft.com/downloads/details.aspx?FamilyID=CAC3E0D2-BEC1-494C-A74E-75936B88E3B5&displaylang=en

在安装 Web 部件模板期间，系统会提示你提供 Microsoft.SharePoint.dll 的位置。此程序集位于 SharePoint（WSS/SPS）服务器上的 Program Files\Common Files\Microsoft Shared\Web Server Extensions\60\ISAPI\ 目录中。你可能需要从安装了 SPS 或 WSS 的服务器上将 Microsoft.SharePoint.dll 复制到你的开发电脑上。
