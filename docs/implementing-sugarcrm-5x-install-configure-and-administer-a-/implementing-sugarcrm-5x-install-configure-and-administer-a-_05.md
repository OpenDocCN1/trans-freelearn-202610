## 前言

SugarCRM 是一款广受欢迎的 **客户关系管理（CRM）** 系统。它既有免费开源版本，也有商业版本，是中小型企业无需投入大量资金即可试用 CRM 系统的理想选择。尽管 SugarCRM 经过精心设计，易于使用，但要取得可衡量的业务成果，仍需进行周密的规划和研究。本书将辛勤获得的 SugarCRM 经验浓缩为一本易于遵循的指南，帮助您充分发挥 SugarCRM 的全部功能。SugarCRM 是一个基于 PHP/MySQL 的大型应用程序，但凭借其丰富的管理界面，无需编程即可从中获得可衡量的收益。

## 本书涵盖的内容

_第 1 章，更好地开展业务_，讨论了企业规模分类及其不同的技术需求。我们还讨论了使用 CRM 技术和系统的好处、CRM 系统的定义、可用的 CRM 选项和部署模型、定制化考量及其对 CRM 系统的重要性，以及 SugarCRM 的概述和使其成为有效 CRM 解决方案的因素。

_第 2 章，没有一种方案适合所有人——按您的方式使用 CRM_，介绍了分析特定企业 CRM 需求的过程，并以 RayDoc 案例研究为例，说明如何将 CRM 功能与企业需求相匹配，并主要关注实施 CRM 系统所追求的业务目标和收益。我们还讨论了如何对 CRM 进行定制的实际问题、在该过程中应寻找什么样的合作伙伴、如何记录定制需求，以及如何管理定制合同。

第 3 章，CRM 部署选项，采用系统化的方法为您的企业做出正确的部署选择。我们详细介绍了 CRM 部署的可用替代方案，包括按需部署、托管、本地部署以及适用于小型组织的共享服务器托管选项。我们还详细讨论了在部署 CRM 系统时需要考虑的一些重要事项，例如选择操作系统（Windows Server 或 Linux）、服务器的硬件配置、所需基础设施、数据备份和安全问题，以及互联网带宽。

第 4 章，SugarCRM 基础，带您游览 SugarCRM，介绍基本 CRM 功能，并展示如何在 SugarCRM 中解释和执行这些功能。我们还讨论了基本 CRM 概念，例如常见的 CRM 流程和术语、创建账户和联系人、将多个联系人关联到单个账户、通过关联数据之间的链接获取客户的完整视图、从线索到机会再到联系人和账户创建和跟踪销售管道、创建和监控销售活动并累积活动历史记录，以及通过参考同事的日历与同事安排活动。

第 5 章，扩展 SugarCRM 系统的业务角色，涉及 CRM 系统在您的企业中超越基本 CRM 功能的角色扩展。我们介绍了删除示例数据并重置数据库以消除默认数据，以及创建和运行电子邮件营销活动。

第 6 章，SugarCRM 生态系统，探讨了许多可用于 SugarCRM Community Edition 的免费插件，这些插件可以将其向多个方向进一步扩展。在此过程中，我们还将了解使用 Module Loader 工具的过程以及升级到 Professional 或 Enterprise Edition 的好处。

第 7 章，管理您的 CRM 实施，讨论了将新 CRM 解决方案引入组织时面临的一些现实挑战。本章的目标是让您了解如何着手并构建自己的 CRM 实施项目。您将发现，成功实施 CRM 的关键之一是循序渐进地进行，因此我们重点介绍了一些重要主题，例如成功实施 CRM 的关键步骤、CRM 培训流程以及持续反馈和改进。

第 8 章，将您的客户与 SugarCRM 关联，讨论了重要主题，例如直接在 CRM 系统中从公共网站捕获客户线索和信息请求、高效跟踪客户服务请求以及相关产品/服务缺陷以提高客户满意度。我们还讨论了与 CRM 系统配合开发客户自助服务门户，以允许客户自行提交或监控其服务案例。

附录 A，在 Linux 上安装 SugarCRM，讨论了在 CentOS Linux 服务器上安装 SugarCRM 所需的步骤。它还讨论了 CentOS Linux 服务器的安装以及 PHP 和 Apache Web 服务器的配置。

附录 B，在 Windows Server 上安装 SugarCRM，提供了在 Windows 服务器上安装 SugarCRM 所需的所有步骤。它还提供了使用 Microsoft SQL Server 和 Internet Information Services (IIS) 安装 SugarCRM 的指南。

附录 C，数据导入和导出，提供了从账户和联系人导入和导出数据的指南。它还讨论了当 CRM 工具在组织中发挥越来越大的作用时，通过导入和导出工具向 CRM 系统插入和提取数据的问题。

附录 D，系统管理员角色，讨论了 SugarCRM 安装中系统管理员的功能和职责，涵盖安装期间和安装后的主题。

附录 E，定制 SugarCRM，讨论了使用各种定制工具（如 Studio、Module builder、Logic hooks、Dropdown Editor 和 Model-View-Controller (MVC)）定制 SugarCRM 的技术。

附录 F，关于 SugarCRM 6.0 的一些说明，简要讨论了 SugarCRM 6.0 及其与 SugarCRM 5.x 的区别以及其优势。

## 本书所需内容

您需要具备基本的计算和数据库术语知识。本书还假设您具备一定的常见计算任务熟练程度，例如浏览文件系统、下载文件等。更高级的任务，例如操作防火墙、DNS 服务器（如有必要）或调整安全性，未在本书中讨论，可能需要合格专业人士的协助。

要跟随示例操作，您需要能够访问一台服务器，在该服务器上可以安装 Linux、Windows 和/或其他必要组件，例如 PHP、Apache 和 MySQL。

## 本书适用人群

如果您是一名中小型企业主/经理，具备合理的 IT 技能，或者是一名系统实施人员或系统管理员，希望为自己实施 SugarCRM（无论是作为第一个 CRM 还是作为现有解决方案的替代品），那么本书适合您。希望扩大对该主题了解的现有 SugarCRM 用户也会发现本书很有价值。使用本书无需编程知识。

## 约定

在本书中，您会发现多种文本样式，用于区分不同种类的信息。以下是这些样式的一些示例及其含义说明。

文本中的代码词显示如下：" Unzip the contents of the installation package to the folder named C:\Program Files\Apache Software Foundation\ Apache2.2\htdocs."

代码块显示如下：

```
<?php
$dictionary['Account']['indices'][] = array('name'
=>'idx_custom', 'type'=>'index',
'fields'=>array('custom_c'));
?>
```

任何命令行输入或输出均写作如下：

```
/echo "0,10,20,30,40,50 * * * * cd </path-to-sugar>;</path-to-php> ./
scheduler.php" | crontab -u apache/
```

**新术语**和**重要词汇**以粗体显示。您在屏幕、菜单或对话框中看到的词，例如，在文本中显示如下："we also see a few buttons just above the main panel — **Edit**, **Duplicate**, **Find Duplicates**, and **Delete**"。

![](img/7ccaf60360d7c8e809d336edbba2263d_19_0.png)

![](img/7ccaf60360d7c8e809d336edbba2263d_19_1.png)





## Reader feedback

读者反馈始终受欢迎。请告诉我们您对本书的看法——您喜欢或不喜欢的地方。读者反馈对我们开发您能真正从中获益的书籍至关重要。

如需发送一般性反馈，只需发送电子邮件至 <feedback@packtpub.com>，并在邮件主题中注明书名。

如果您有需要并希望我们出版的书籍，请通过 www.packtpub.com 上的 SUGGEST A TITLE 表单与我们联系，或发送电子邮件至 suggest@packtpub.com。

如果您在某个领域有专长，并有兴趣撰写或参与编写书籍，请参阅 www.packtpub.com/authors 上的作者指南。

## Customer support

既然您是 Packt 图书的自豪拥有者，我们有许多资源可帮助您充分利用您的购买。

## Errata

尽管我们已竭尽全力确保内容的准确性，但错误难免会发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激如果您能向我们报告。这样做，您可以避免其他读者感到沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/support，选择您的书籍，单击勘误提交表单链接，并输入您的勘误详细信息。一旦您的勘误得到验证，您的提交将被接受，勘误将上传到我们的网站，或添加到该书名下 Errata 部分的现有勘误列表中。任何现有勘误都可以通过从 http://www.packtpub.com/support 选择您的书籍来查看。

## Piracy

互联网上受版权保护材料的盗版是所有媒体中持续存在的问题。在 Packt，我们非常重视对我们的版权和许可的保护。如果您在互联网上以任何形式发现我们作品的非法副本，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过 <copyright@packtpub.com> 与我们联系，并提供涉嫌盗版材料的链接。

感谢您帮助保护我们的作者以及我们为您提供有价值内容的能力。

## Questions

如果您在书籍的任何方面遇到问题，可以通过 <questions@packtpub.com> 联系我们，我们将竭力予以解决。
