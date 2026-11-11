# ASP.NET 2.0 增强 Microsoft 内容管理服务器：Microsoft 内容管理服务器开发中的 ASP.NET 2.0 母版页、主题、站点导航和成员资格提供程序模型




## 致谢

### 作者

Spencer Harbar
Lim Mei Ying
Stefan Goßner

### 审阅者

Andreas Klein
Mick Badran
Chester Ragel
Joel Ward

### 开发编辑

Douglas Paterson

### 技术编辑

Niranjan Jahagirdar

### 编辑经理

Dipali Chittar

### 索引员

Mithil Kulkarni

### 校对员

Chris Smith

### 生产协调员

Manjiri Nadkarni

### 版式与插图

Shantanu Zagade

### 封面设计师

Shantanu Zagade

## 关于作者

**Spencer Harbar**，微软 .NET 的 MCSD、MCSE 以及 MCMS 的 MVP，拥有超过十二年的商业经验，负责为欧洲一些最大组织的基于 Web 的应用程序和托管平台进行架构、设计、开发、部署和运营支持。

Spencer 维护 www.mcmsfaq.com：一个 MCMS 资源门户，并在公共新闻组中活跃。他对 MCMS 的经验可追溯至 NCompass Resolution 时代，并参与了许多企业级实施。

目前作为独立顾问工作，Spencer 提供企业内容管理和门户系统架构、设计、开发和部署解决方案、应用程序安全最佳实践、威胁建模，以及基于 Windows Server System 的高可用性托管平台的实现。

产品专长包括 MCMS、SharePoint 技术、IIS、SQL Server、Commerce Server 和 BizTalk Server、Windows 安全、PKI 和高可用性。

选定的客户包括 Barclays Bank、Scottish Power plc、HBOS、Microsoft、The Royal Bank of Scotland、Scottish Enterprise、Centrica、Clifford Chance、The Automobile Association 和 BASF。

Spencer 居住在英国爱丁堡。

**Lim Mei Ying** 是 Avanade 的高级顾问，在企业级搭建 MCMS 系统方面拥有丰富的经验。她花费了大量时间研究该产品的注意事项，并喜欢寻找解决 MCMS 相关问题的新方法。

她积极为新闻组社区做出贡献，并是内容管理服务器的微软最有价值专家。Mei Ying 生活在阳光明媚的新加坡岛屿，并在 `http://meiyinglim.blogspot.com` 博客。她还合著了早期书籍 _使用 Microsoft Content Management Server 构建网站_（ISBN 1-904811-16-7，Packt Publishing）。

> 感谢我的丈夫 Louis 在漫长的写作月里给予的迫切需要的支持。特别感谢我的家人和朋友的鼓励。

**Stefan Goßner** 在微软开发者支持部门担任升级工程师。他为客户提供与微软互联网服务器产品相关问题的技术解决方案。Stefan 对 MCMS 的各个领域都有广泛的了解。

他对新闻组的贡献帮助了许多人全球各地的公司实施 MCMS 解决方案，以至于有人说如果你不认识 Stefan，那你可能刚接触 MCMS。

Stefan 在微软网站上维护了一个庞大的 MCMS 2002 常见问题解答，并在他的个人博客 `http://blogs.technet.com/stefan_gossner` 上提供 MCMS 技巧。

Stefan 还撰写了书籍 _使用 Microsoft Content Management Server 构建网站_（ISBN 1-904811-16-7，Packt Publishing），以及 _高级 Microsoft Content Management Server 开发_（ISBN 1-904811-53-1，Packt Publishing）。

他居住在德国慕尼黑，可通过 `webmaster@stefan-gossner.de` 联系。

> 我想感谢我的女朋友 Michaela，在为本书的写作、审阅和编码的数月里给予的支持。还要非常感谢全球 MCMS 支持团队的朋友们以及美国 MCMS 产品团队的朋友们。

## 关于审阅者

**Andreas Klein** 于 1981 年进入 PC 业务，在 1990 年加入微软之前担任了数年的顾问。自那以后他一直举办编程培训。在技术方面，他涵盖了许多不同的领域，包括 Win16（Windows 2.x、3.x）和 Win32 应用程序和驱动程序编程，帮助客户管理运行 Windows 9x、Windows NT 及后续版本，甚至 Exchange 4.0 - 2000 的系统。

他目前的重点是 Web 服务器场景（IIS、MCMS 2002）、DHTML 编程，以及 IT 范围内的 PKI/安全概念。

**Mick Badran** 已从事微软技术课堂培训超过九年，并在各种语言方面拥有超过 12 年的商业开发经验。Mick 已为微软在 CMS、SPS 和 BizTalk 领域提供咨询超过五年。他还专门从事这些领域的定制培训。

他在各种微软活动上演讲，如 TechEd 和安全峰会，并是 BizTalk MVP。

他可以通过 `mickb@breezetraining.com.au` 联系，并很乐意听取您的反馈。

**Joel Ward** 担任 Web 开发人员和技术经理。他曾立志成为建筑师，但在大学中途改变方向，转而从宾夕法尼亚州立大学获得整合艺术学位。他的职业生涯经历了设计、编程、可用性和可访问性的历程。

Joel 自 2001 年首次发布以来一直乐于使用 MCMS。多年来，Joel 在新闻组中活跃，并因其在 MCMS 社区的工作而被认可为微软最有价值专家。

他喜欢好的挑战，其中包括使用 ASP.NET、MCMS 和 SharePoint 的项目。

Joel 住在美国弗吉尼亚联邦。你可以访问他的网站 `http://www.wardworks.com/joel/`。


## 前言
《使用 ASP.NET 2.0 增强 Microsoft Content Management Server》深入探讨了将 ASP.NET 2.0 的关键功能（如母版页、站点导航、主题和皮肤以及成员资格提供程序模型）与 Microsoft Content Management Server 2002 (MCMS) Service Pack 2 集成。

每章都建立在前一章的基础上，逐步介绍 MCMS 开发人员可用的这些新功能，并构建一个示例站点，该站点与我们在 Packt Publishing 于 2005 年 1 月出版的前一本书《使用 Microsoft Content Management Server 构建网站》（ISBN 1-904811-16-7）中介绍的站点类似。

## 本书涵盖内容
第 1 章指导您在单台开发工作站上安装和配置 MCMS 2002 Service Pack 2 (SP2)，以及 SQL Server 2005 和 Visual Studio 2005。设置 SP2 开发环境有两种方法：从之前的 SP1a 安装升级，或从头开始构建包含 SP2 的全新安装。本章将介绍这两种方法。

在第 2 章中，我们花一些时间熟悉 MCMS Service Pack 2 开发环境，该环境与我们习惯的 MCMS 和 Visual Studio 早期版本略有不同。我们将介绍这些变化以及在 SP2 开发环境中工作的一些提示，例如创建自定义 MCMS Visual Studio 2005 模板。

第 3 章向您介绍 ASP.NET 2.0 引入的最佳新功能之一——母版页，它允许开发人员在应用程序内的页面中强制执行统一的布局和行为。乍一看，许多母版页概念与 MCMS 模板相似，但在 MCMS 应用程序中利用母版页可以获得诸多益处。本章提供了使用母版页的优势概述，以及在 MCMS 应用程序中实现它们的逐步指南，在那里它们成为主模板！

第 4 章涵盖了新的 ASP.NET 2.0 导航提供程序模型和控件，以及如何将它们集成到您的 MCMS 应用程序中。开发站点导航控件通常会让开发人员陷入两难境地。Visual Studio 的早期版本未提供任何即用型导航控件。开发人员必须在花费大量时间从头构建控件或扩大项目预算购买现成软件之间做出选择。

在第 5 章中，我们将了解如何通过使用主题高效地为 MCMS 站点应用统一的外观和感觉。我们将创建皮肤和层叠样式表，并演示它们如何协同工作以定义站点的外观。最后，我们将讨论使主题在 MCMS 站点中正确工作的必要自定义。

第 6 章涵盖了成员资格提供程序模型，这是 ASP.NET 2.0 引入的关键新概念之一，它使开发利用第三方或自定义成员资格存储的 Web 应用程序变得更加容易。此外，ASP.NET 2.0 附带了与角色成员资格相关的多个身份验证控件，大大减少了在应用程序中实现表单身份验证及相关功能所需的代码量。本章展示了如何使用这些功能改进 Forms Authentication 的实现，并为“账户映射”场景提供更优雅的解决方案，在该场景中身份验证针对外部存储进行，而账户则映射到 Windows 账户以用于 MCMS 授权。

在第 7 章中，我们提供了与 ASP.NET 2.0 和 MCMS SP2 配合使用的多个提示以及代码示例，并为考虑迁移到即将推出的 Microsoft Office SharePoint Server 2007 的用户提供实施建议。

## 本书所需条件
本书面向熟悉我们前一本书《使用 Microsoft Content Management Server 构建网站》中介绍的材料并对 C# 有扎实掌握的 MCMS 开发人员。要使用本书，您需要具备以下访问权限：

- Visual Studio 2005（任意版本）。
- Microsoft Content Management Server 2002 Service Pack 2（任意版本）。MCMS 的 120 天试用版可从 http://www.microsoft.com/cmserver 获取。



我们在第 1 章中介绍安装的各种先决条件。本书中呈现的所有示例均使用 Microsoft Windows XP Professional Service Pack 2。

## 约定

在本书中，您会找到多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例，以及它们的含义说明。

代码有三种样式。文本中的代码词显示如下："我们可以通过使用 `include` 指令来包含其他上下文。"

代码块将如下设置：

```
public string FirstName
{
    get
    {
        return txtFirstName.Text.Trim();
    }
}
// Last Name
public string LastName
{
    get
    {
        return txtLastName.Text.Trim();
    }
}
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目会加粗：

```
public string FirstName
{
    get
    {
        **return txtFirstName.Text.Trim();**
    }
}
// Last Name
public string LastName
{
    get
    {
        return txtLastName.Text.Trim();
    }
}
```

任何命令行输入和输出均按如下方式书写：
xcopy "MCMS_INSTALL_PATH\DevTools\NewProjectWizards80\Visual Web Developer" "PATH_TO_MY_DOCUMENTS_FOLDER\Visual Studio 2005\Templates\ProjectTemplates\Visual Web Developer"/E

**新术语**和**重要词语**以粗体字体引入。您在屏幕上看到的词语，例如菜单或对话框中的词语，在我们的文本中如下所示："点击 **Next** 按钮会将您带到下一个屏幕"。

![](img/18a0aa8d36427a4c069b30410f998b8e_14_0.png)

![](img/18a0aa8d36427a4c069b30410f998b8e_14_1.png)

## 读者反馈

欢迎读者反馈。请告诉我们您对本书的看法，您喜欢或可能不喜欢的部分。读者反馈对我们开发您真正受益的书名非常重要。
如需发送一般反馈，只需发送电子邮件至 <feedback@packtpub.com>，并在邮件主题中注明书名。
如果您需要一本书并希望我们出版，请在 `www.packtpub.com` 上的 **建议标题** 表单中发送说明，或发送电子邮件至 `suggest@packtpub.com`。
如果您在某个主题方面有专业知识，并且有兴趣撰写或为一本书做出贡献，请参阅 `www.packtpub.com/authors` 上的作者指南。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项服务来帮助您充分利用您的购买。

## 下载本书的示例代码

访问 `http://www.packtpub.com/support`，并从标题列表中选择本书
以下载本书的任何示例代码或额外资源。可供下载的文件
将显示出来。

> 可下载文件包含如何使用它们的使用说明。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果您在我们的书籍中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。通过这样做，您可以避免其他读者的沮丧，并帮助改进本书的后续版本。如果您发现任何勘误，请访问 `http://www.packtpub.com/support`，选择您的书籍，点击 **提交勘误** 链接，并输入勘误的详细信息。一旦您的勘误得到验证，您的提交将被接受并添加到现有勘误列表中。现有勘误可以通过从 `http://www.packtpub.com/support` 选择您的标题来查看。

## 问题

如果您对书籍的某些方面有问题，可以联系我们 `questions@packtpub.com`，我们将尽力解决。
