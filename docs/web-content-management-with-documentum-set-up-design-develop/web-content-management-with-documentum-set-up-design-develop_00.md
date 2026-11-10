# Documentum Web 内容管理：设置、设计、开发和部署 Documentum 应用：关于 Documentum Web 内容管理的简明实用信息，助您充分利用该系统

## 鸣谢

| **作者** Gaurav Kathuria | **索引员** Niranjan Jahagirdar |
|---|---|
| **开发编辑** Douglas Paterson | **校对员** Chris Smith |
| **助理开发编辑** Nikhil Bangera | **制作协调员** Manjiri Nadkarni |
| **技术编辑** Ashutosh Pande | **版式和插图** Shantanu Zagade |
| **编辑经理** Dipali Chittar | **封面设计师** Shantanu Zagade |

## 关于作者

Gaurav Kathuria 于2000年毕业于 I.I.T. Kharagpur，获得化学工程 B. Tech. (Hons.) 学位，此后在 IT 服务、产品开发到软件咨询等多样化的软件领域表现出色。

他在利用面向对象语言和技术如 Java/J2EE 和 Documentum 设计、开发和管理软件系统方面拥有丰富的经验。

他于2001年开始使用 Documentum 4i，此后在架构/设计复杂的 Documentum 4i 和 5x 项目方面拥有丰富的经验。

他还在工作过的许多组织中就 Documentum 系统架构、基础知识和 Web Publisher 进行了内部培训。

本书献给

上帝：祂一直 showered his choicest blessings on me 并给予我远超我所想要的生活。我感谢祂为我所做的一切。

我的家人：我的父亲（Mr. P.N. Kathuria）一直是我生命中的指路明灯，指导并引领我度过顺境与逆境。他在所有事业中都极其勤勉和诚实；我从他身上学到了很多，并且仍在学习。

我的母亲（Mrs. Sarita Kathuria）无私地将一生奉献给我们的小家庭。她一直是我困境中的依靠，也是当他人不认可我时最赞扬我的人。

我的妹妹（Ms. Gunjan Kathuria）是我一直想要的甜美小妹。她的关爱和照顾为我的生活增添了新的意义。

我的妻子（Mrs. Gunjan Grover）用对每个人的爱与尊重装点我们的家。她的存在填补并完成了我生命中缺失的部分...

我的朋友：Neeraj Jain, Nisha, Hima, Nishant Anchal, 和 Abhishek Singh，他们一直陪伴在我身边，让这个世界变得更美好。

Documentum 团队成员：Mansoor Sheikh, Arnab Ghosh, Amit Kapur, Prashant Shukla, Gajendra Sahu, Gurmeet Singh, Prasen Misra, Tanveer Haider, Arpana Bansal, Preeti Dua, Kapil Bharati, Akash Narang, Kesavan, Usha Parolkar, Anjali Nanda, 以及我曾在各种 Documentum 项目中合作的其他软件专业人士。他们都在某种程度上成为我的灵感来源。

我感谢你们所有的爱与支持！

## 致谢

Dr. Louay Fatoohi, Dr. Douglas Paterson 以及 Packt Publishing Ltd. 的全体团队，因在最严格的时间范围内出版本书而给予我荣誉。

Johnson Stephen：我的挚友、同事和软件测试大师。他“无意中”引导我撰写本书，作为未来建立自己并创办咨询公司的先导。

M. Scott Roth：'A Beginner's Guide to Developing Documentum Desktop Applications' 的作者。

Scott 赞扬我撰写关于 Documentum 技术的决定，并不断提供急需的支持和热情。

Anil Baid：'Solutions Infosystems' 的所有者和负责人。他一直是我极大的帮助，没有他本书绝不会面世。

Rakesh Dahiya：'Solutions Infosystems' 的设施经理，他经常就各种出版渠道以及行业技巧提供指导。

Ashwin Razdan：Media Manager, whatistesting.com；一位极具多才多艺的人物，他通过提供急需的方向和支持帮助我将本书打磨到合适的标准。

Sachin Jain：'Solutions Infosystems' 的财务和法律负责人，他宝贵的建议使我在撰写本书过程中避开了许多困难局面。

Pankaj Jain 和 Pradeep Gautam（'Econsultants India'）。

我真诚地向可能遗漏姓名的各位道歉。你们对我都很重要。

## 前言

作为企业内容管理领域的世界领先者之一，EMC Documentum 应用系列帮助您在一个单一存储库中管理各种类型的内容。借助 Web 内容管理应用套件，您可以高效地管理网站的内容和底层流程，并确保其响应业务需求。

充分发挥该系统的强大功能似乎令人望而生畏，但本书将帮助您实现这一目标。凭借易于遵循的示例，本书将带您走上最简单、最直接的成功之路。在此过程中，您将学到只有经验丰富的专业人士才知道的见解。

本书充实了实用示例，将让您亲身实践 Documentum 的强大功能，以提升技能和信心。您将看到处理系统复杂性的技巧和窍门，并避免浪费时间的常见错误。

## 本书涵盖内容

第 1 章：本章讨论内容管理系统需求，并对 Documentum 进行介绍。

第 2 章：本章介绍内容服务器并讨论与 Documentum 相关的基本概念，如 Docbases、DocApps、DocBrokers 和对象。本章还涉及内容服务器的版本控制功能，并介绍生命周期和工作流。

第 3 章：本章涵盖 Documentum 中的高级概念，如 DMCL、DFC、BOF、WDK、ACL、呈现、注册表、数据字典、方法和作业。

第 4 章：本章介绍 Documentum 产品套件。

第 5 章：本章讨论内容服务器 5.2.5 和服务包 2 的安装。

第 6 章：本章讨论创建 Docbase 并在新创建的 Docbase 上安装 Web Publisher 文件的详细步骤。它还讨论如何启动和停止 Docbases 和 DocBrokers，并最后讨论 dmcl.ini 和 server.ini 文件中的一些重要键。

第 7 章：本章介绍为在新 Docbase 中创建的文档发布而设置站点缓存服务（SCS）组件。

第 8 章：本章简要介绍 Documentum Application Builder 作为创建和管理 Documentum DocApps 的客户端工具，然后介绍其安装的详细步骤。

第 9 章：本章讨论 Documentum Administrator 和 Web Publisher 的安装。

第 10 章：本章介绍 Documentum 应用的设计，然后涉及 Web Publisher 模板、规则文件和演示文件架构。

第 11 章：本章讨论 Documentum 对象类型及其属性。它还讨论值辅助以及在 Documentum 中创建和查询注册表。

第 12 章：本章详细介绍 Documentum 别名集、权限集（ACL）和生命周期。

第 13 章：本章提供在 Web Publisher 中创建示例模板的详细说明。

第 14 章：本章介绍规则文件并介绍在 Web Publisher 中创建规则文件以及设置调用规则编辑器的首选项，并讨论可用的规则文件小部件。

第 15 章：本章介绍演示文件并讨论创建它们的详细步骤以及在 Web Publisher 中将它们与模板文件关联。本章还讨论通过 XDQL 执行 DQL 查询以及如何自动对活动内容文件重新应用演示文件以在 Docbase 中创建更新的呈现。

第 16 章：本章讨论 Web Publisher 中的文件夹映射及其限制，并提供多个配置文件夹映射的示例，使用各种属性匹配机制、单个和重复属性以及运行时动态文件夹映射。

第 17 章：在详细示例的帮助下，本章讨论如何使用指令文件从内容 XML 文件中删除 XML 元素、向其中添加新的 XML 元素以及更新 XML 元素现有值。

第 18 章：本章讨论自动属性提取（APE），还讨论使用 APE 填充重复属性以及进行双向属性提取。

第 19 章：本章详细讨论工作流和工作流模板，并包含创建自定义工作流的示例。

第 20 章：本章提供测试上述创建的自定义工作流的详细步骤。

第 21 章：本章详细讨论站点缓存服务（SCS），并解释如何在 Documentum Administrator 中创建站点发布配置以定义使用 SCS 发布时的源主机和目标主机参数。它还讨论了一种基于浏览器的简单机制，用于查看 SCS 源发布操作的状态。

第 22 章：通过详细步骤，本章讨论如何使用 Documentum Administrator 中的站点发布配置在 Documentum 中设置 WebView。

第 23 章：本章讨论 Documentum 基础类（DFC），并包含有关如何以编程方式使用 DFC 创建 Docbase 会话、在 Docbase 文件柜中创建和链接文件以及在 Documentum 中创建用户的详细示例

第 24 章：本章讨论 Web 开发工具包（WDK）框架，并附有其配置和自定义的示例。

第 25 章：本章讨论在不同的测试和生产环境中部署 Documentum 应用。

第 26 章：本章解释 DQL 查询和服务器 API 命令作为检查 Documentum Docbase 的便捷工具的使用。

附录 A：本附录包含基于本书所涵盖内容的常见问题解答（FAQs）答案。

附录 B：本附录包含在 Documentum 版本 5.3 中添加的功能和增强列表。

## 您需要准备的内容

为了充分利用本书，您需要访问 Documentum 产品套件的工作安装。

本书针对在 Windows 环境下运行的 Documentum 产品套件版本 5.2.5 SP2 编写，其中包括 SQL Server 2000 数据库服务器。您还需要安装 Apache Tomcat 4.1.30 平台和 Apache Ant 1.6.5。

## 惯例

在本书中，您会找到多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义说明。

代码有三种样式。文本中的代码词显示如下："外部演示文件为嵌入在 HTML 内容文件 <body></body> 元素中的内容提供标准框架和包装。"

代码块将设置如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<ROOTTAG>
    <TITLEOFPAGE/>
    <SHORTDESC/>
</ROOTTAG>
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目将被加粗：

```
<?xml version="1.0" encoding="UTF-8"?>
<ROOTTAG>
    <TITLEOFPAGE/>
    <SHORTDESC/>
</ROOTTAG>
```

任何命令行输入和输出写成如下：

```
DQL> create dm_document object set object_name = 'TestDocumentCreated_via_DQL', setfile 'C:\Test\testing_dql.xml' with content_format = 'xml'
```

新术语和重要词语以粗体字体引入。例如，您在屏幕、菜单或对话框中看到的词语，会在我们的文本中这样出现："点击 Next 按钮会带您进入下一个屏幕"。

警告或重要说明会以如下框形式出现。



提示和技巧如下所示。

在 5.3 版本中对 Documentum 套件所做的更新将以下列标题标出：

## Documentum 5.3 更新

在 5.3 版本中对 Documentum 所做的更新将显示在此处。

## 读者反馈

我们非常欢迎读者的反馈。请告诉我们您对本书的看法，您喜欢或可能不喜欢的部分。读者反馈对我们开发您真正能充分利用的书名非常重要。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中注明书名。

如果您需要一本书并且希望我们出版，请在 www.packtpub.com 上的 SUGGEST A TITLE 表单中给我们留言，或发送电子邮件至 suggest@packtpub.com。

如果您在某个主题方面有专长，并且有兴趣撰写或为书籍做出贡献，请参阅 www.packtpub.com/authors 上的作者指南。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项帮助以助您充分利用您的购买。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。通过这样做，您可以为其他读者节省挫败感，并帮助改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/support，选择您的图书，点击 Submit Errata 链接并输入勘误详情。一旦您的勘误得到验证，您的提交将被接受并将勘误添加到现有勘误列表中。现有勘误可通过从 http://www.packtpub.com/support 选择您的书名进行查看。

## 问题

如果您在本书的某些方面遇到问题，可以联系我们 questions@packtpub.com，我们将尽力解决。
