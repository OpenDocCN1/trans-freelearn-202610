# Microsoft IIS 10.0 秘籍：安装、配置和管理 IIS 10.0 的 60 多个技巧

## 关于作者

**Ashraf Khan** 担任网络操作系统部门主管，拥有生产系统、网络管理和解决方案架构的多元背景。他拥有 MCSE-Server、MCSE-Messaging、MCSE-Cloud、CCNA、ITIL 和 Prince2 认证。他在 IT 基础设施、私营和政府部门等领域拥有超过 13 年的经验，工作地点在印度和阿联酋，并为美国及全球的物理和虚拟服务器环境提供远程支持，负责 IT 管理、实施、规划、灾难恢复、测试和技术支持。

我谨向帮助我完成本书的许多人以及所有提供支持、讨论问题、阅读、撰写、提供评论、允许我引用其言论，并在编辑、校对和设计方面提供帮助的人表示感谢。

我要感谢我的编辑 Heramb Bhavsar 和 Sharon Raj 以及审阅者 Anwarul Haque，使我能够出版这本书。最重要的是，我要感谢我的妻子 Shama 和我的家人，他们在所有让我远离他们的时刻都给予了我支持和鼓励。对他们来说，这是一段漫长而艰难的旅程。

我要感谢 Sharon Raj 在选题和编辑过程中的帮助。感谢 Packt 对我的鼓励。

最后但并非最不重要，我请求原谅所有多年来与我在一起却未被我提及姓名的人。

## 关于审阅者

**Anwarul Haque** 自 2004 年起专业从事应用程序开发。Haque 在企业和消费领域担任顾问超过 12 年。他自 2007 年起获得 Microsoft SharePoint 2007 Application Development 认证，自 2014 年起获得 Microsoft Dynamics NAV 2013 C/Side Development 认证，自 2014 年起获得 Microsoft Certified Professional Developer 认证。他还曾在 SharePoint 2010 和 2013 本地版、在线 SharePoint Office 365 以及 IIS 服务器管理方面工作，具备管理级别的主机管理技能。他在各种平台和技术应用上管理过 Web 服务器，这些服务器托管在 Windows IIS Server、Azure Server 和 Unix Server 上。除了 IIS Server 外，他还管理并托管了 Apache Tomcat Server 上的网站。

在开发领域，他曾使用各种 IDE、工具和技术工作，例如用于 SharePoint 开发的 Microsoft Office Designer 应用程序、Microsoft CRM Portal、ASP.Net、VB.Net、ADO.Net、WCF、SSRS、ASP 3.0、Entity Framework、SQL Server 2005、2008、2012、2015、Oracle、Visual Basic 6.0 & VBA 宏、VB Script、JavaScript、Ajax、jQuery、动态 Excel 报表和图表，以及 Microsoft Office 产品的附加组件开发。他还使用过 Microsoft Visual SourceSafe、SVN 和 IBM Rational ClearCase 等版本控制工具。

作为一名专业人士，他曾在 Matrix InfoSoft、MetaOption LLC 等中小型企业工作，目前在总部位于新德里的 GCE Solutions 担任软件分析师。

您可以在 LinkedIn 上关注他：https://www.linkedin.com/in/anwarul-haque-b4824b21/。

*我谨向我可爱的妻子 Zeenat Haque 感谢她的耐心、支持和理解！*

## 前言

IIS 10.0 是 Internet Information Services（IIS）的最新版本，IIS 是由 Microsoft 开发的可扩展 Web 服务器。IIS 10.0 随 Windows Server 2016 和 Windows 10 操作系统一起推出。IIS 10.0 的新功能包括 HTTP/2、Nano Server 上的 IIS、通配符主机头和 PowerShell 5 Cmdlets。IIS 10.0 支持所有先前功能：HTTP、HTTPS、SMTP 和 SNMP、FTP 和 FTPS。

## 本书涵盖内容

第 1 章，集成 IIS 10.0 与 Windows Server 2016，首先介绍有关 IIS 10.0 的一些基本信息。之后，我们将把 IIS 10.0 与 Windows Server 2016 集成，并配置和测试我们的第一个示例网页。

第 2 章，在 IIS 10.0 中创建应用程序池，解释应用程序池、不同版本应用程序池的安装、应用程序池的创建和配置。我们将上传和测试 .NET 3.5 和 .NET 4.0 Web 应用程序。

第 3 章，在 IIS 10.0 上托管多个网站，首先介绍在同一 IIS 10.0 实例上进行多 Web 托管，我们将通过主机名（域名）访问这些网站。我们将为网站配置专用 IP 地址。

第 4 章，在 IIS 10.0 中构建虚拟目录，首先介绍虚拟目录的创建及其在现有网站中的使用。如果您有需要网站中应用程序池的不同 Web 应用程序，您可以创建虚拟目录并添加所需的应用程序池。

第 5 章，在 IIS 10.0 上安装 HTTP/2，解释 HTTP/2 以及 HTTP/2 的安装、配置和测试。

第 6 章，让您的通配符主机运行起来，展示如何创建以主域名为前缀的子域名 demo，例如 demo.v2mysite.com。我们可以为父域创建不同的名称和无限的通配符主机。

第 7 章，在 Nano Server 上部署 IIS 10.0，首先介绍在 Nano Server 上安装 IIS 10.0。我们将远程连接 Nano Server，在 PowerShell 中导入 IISAdministration 模块，并在 Nano Server 上创建和管理网站。

第 8 章，使用 PowerShell Cmdlets 配置 IIS 管理，使用 PowerShell 在 IIS 服务器上执行管理任务。

第 9 章，在 Nano Server 上的 IIS 中启用 ASP.NET Core，教授如何在 Nano Server 上的 IIS 中启用 ASP.NET 并创建虚拟目录。

第 10 章，安装和配置 SSL 网站，首先介绍第三方 SSL 证书的安装；我们将创建本地自签名证书并对其进行测试。

第 11 章，将 IIS 10.0 扩展到 FTP，展示如何在 Windows Server 2016 上安装 FTP 服务器角色。我们创建和配置 FTP 站点。

第 12 章，在 IIS 10.0 上保护您的网站，解释 IIS 10.0 安全性和可用的安全配置，例如 IP、域和 URL。

第 13 章，管理和故障排除 IIS 10.0，解释 IIS 10.0 运行状况和性能的配置和管理。

## 阅读本书所需条件

为了使用 IIS 10.0，应具备以下硬件和软件：

- **所需软件（及版本）**：IIS 10.0、FileZilla FTP 客户端软件、Visual Studio 2015 或更高版本、HTML 文件、PowerShell 5 cmdlets
- **硬件规格**：4 GB RAM、300 GB HDD、2 个 v处理器。
- **所需操作系统**：Windows Server 2016、Nano Server 2016 和 Windows 10

## 本书适合人群

本书适合那些一直担任 IIS 管理员、系统管理员、网站管理员和 Web 开发人员的人，例如网站构建者、后端开发人员和前端开发人员，以及那些渴望了解开始使用 IIS 10.0 后会遇到什么的人。

## 章节

在本書中，您会经常看到几个标题（准备工作、如何操作...、其工作原理...、更多内容...、另请参阅）。

为了清晰地说明如何完成一个配方，我们按如下方式使用这些章节：





### 准备工作

本节介绍本食谱中您将遇到的内容，并描述为本食谱设置任何软件或任何初步设置所需的操作。

### 如何操作...

本节包含遵循本食谱所需的步骤。

### 它是如何工作的...

本节通常包含对前一节中发生情况的详细解释。

### 还有更多...

本节包含有关食谱的附加信息，以便让读者更了解该食谱。

### 另请参阅

本节提供指向食谱其他有用信息的帮助链接。

## 惯例

在本书中，您会找到多种文本样式，用以区分不同类型的信息。以下是这些样式的一些示例及其含义的解释。

正文中出现的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下："Create a new user for JIRA in the database and grant the user access to the jiradb database we just created using the following command."

代码块设置如下：

```
<Contextpath="/jira"docBase="${catalina.home}
/atlassian- jira" reloadable="false" useHttpOnly="true">
```

任何命令行输入或输出都写成如下形式：

```
mysql -u root -p
```

新术语和重要词语以粗体显示。您在屏幕上看到的词语，例如菜单或对话框中的词语，在文本中如下所示："Select **System info** from the **Administration** panel."

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_20_0.png)

警告或重要说明以如下框显示。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_20_1.png)

提示和技巧如下所示。

## 读者反馈

我们始终欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢或不喜欢的内容。读者的反馈对我们非常重要，因为它有助于我们开发您真正能充分利用的书籍。

如需发送一般反馈，只需发送电子邮件至 `feedback@packtpub.com`，并在邮件主题中提及本书的标题即可。

如果您在某个主题方面具有专业知识，并且有兴趣撰写或为一本书做出贡献，请参阅我们的作者指南 `www.packtpub.com/authors`。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项服务来帮助您充分利用您的购买。

### 下载示例代码

您可以从您的账户在 `http://www.packtpub.com` 下载本书的示例代码文件。如果您在其他地方购买了本书，您可以访问 `http://www.packtpub.com/support` 并注册以便直接将文件发送到您的邮箱。

## 前言

您可以通过以下步骤下载代码文件：

+   1. 使用您的电子邮件地址和密码登录或注册我们的网站。
+   2. 将鼠标指针悬停在顶部 **SUPPORT** 选项卡上。
+   3. 点击 **Code Downloads & Errata**。
+   4. 在 **Search** 框中输入本书的名称。
+   5. 选择您要下载代码文件的书籍。
+   6. 从下拉菜单中选择您购买本书的渠道。
+   7. 点击 **Code Download**。

您也可以通过在 Packt Publishing 网站的本书网页上点击 **Code Files** 按钮来下载代码文件。此页面可以通过在 **Search** 框中输入书名来访问。请注意，您需要登录您的 Packt 账户。

下载文件后，请确保使用最新版本解压缩或提取文件夹：

+   - WinRAR / 7-Zip for Windows
+   - Zipeg / iZip / UnRarX for Mac
+   - 7-Zip / PeaZip for Linux

本书的代码包也托管在 GitHub 上，地址为 `https://github.com/PacktPublishing/Microsoft-IIS-10Dot0-Cookbook`。我们还在 `https://github.com/PacktPublishing/` 上托管了来自我们丰富图书和视频目录的其他代码包。快来看看吧！

### 下载本书的彩色图片

我们还为您提供了一份 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图片。彩色图片将帮助您更好地理解输出的变化。您可以从 `https://www.packtpub.com/sites/default/files/downloads/MicrosoftIIS10Dot0Cookbook_ColorImages.pdf` 下载此文件。
