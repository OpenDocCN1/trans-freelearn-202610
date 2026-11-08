
Francesco Corti

## Pentaho 8
面向
Java 开发人员
的报表

使用报表工具创建像素级精确的分析报表

![](img/9337ac94537bf7e092e1ea6531dce8fd_0_0.png)

Packt

## Pentaho 8 Reporting for Java Developers

使用报表工具创建像素级精确的分析报表

Francesco Corti

![](img/9337ac94537bf7e092e1ea6531dce8fd_2_0.png)

BIRMINGHAM - MUMBAI

## Pentaho 8 Reporting for Java Developers

版权所有 © 2017 Packt Publishing 保留所有权利。未经出版者事先书面许可，本书的任何部分不得以任何形式或任何手段复制、存储于检索系统或传播，但用于评论或文章中简短引用的情况除外。

在编写本书时已尽最大努力确保所提供信息的准确性。然而，本书所含信息按“原样”提供，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商不对因本书直接或间接造成或声称造成的任何损害承担责任。

Packt Publishing 已尽力通过适当使用大写字母提供本书中提及的所有公司和产品的商标信息。然而，Packt Publishing 不能保证该信息的准确性。

首次出版：2017 年 9 月

生产参考：1120917

由 Packt Publishing Ltd. 出版
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-78829-899-5

www.packtpub.com

## 致谢

| **作者** | **校对编辑** |
|---|---|
| Francesco Corti | Safis Editing |
| **审阅者** | **项目协调员** |
| Ruben Oliva Ramos
Juan Tomas Oliva Ramos | Nidhi Joshi |
| **委约编辑** | **校对员** |
| Amey Varangaonkar | Safis Editing |

| **收购编辑**<br>Viraj Madhav | **索引员**<br>Tejal Daruwale Soni |
|---|---|
| **内容开发编辑**<br>Cheryl Dsa | **图形**<br>Tania Dutta |
| **技术编辑**<br>Dinesh Pawar | **生产协调员**<br>Arvindkumar Gupta |

## 关于作者

**Francesco Corti** 是一位充满热情的软件解决方案顾问，喜欢在开发、销售和客户团队中工作。作为一名软件工程师，他经常参与售前演示、公开演讲和 IT 课程。软件开发、架构设计以及在 ECM/BPM 和 BI 领域的解决方案定义是他最感兴趣的领域。他在近 20 年的经验中完成了数十个项目，从非常小的项目到更复杂的项目。

在 Alfresco 任产品布道师，Francesco 在开发者社区代表著名的开源 ECM。除了帮助开发者采用 Alfresco 技术外，他还经常通过演讲、文章、博客、用户演示、录制演示或创建示例项目来帮助 Alfresco 改善开发者体验。

他是 Alflytics（之前名为 Alfresco Audit Analytics and Reporting）的发明者和主要开发者，该业务智能解决方案基于 Alfresco ECM，完全基于 Pentaho 套件构建。他撰写了包含 40 多个视频的 Pentaho Reporting 视频课程，以及关于 Pentaho Reporting Designer 和 SDK 的课程。

Francesco 在企业内容管理解决方案方面拥有专业和主要经验，使用 Alfresco ECM 和 Hyland OnBase（他是 OnBase 认证安装人员）；使用 Activiti、JBPM 和 Hyland OnBase 的业务流程管理解决方案；使用 Ephesoft、Hyland OnBase 和定制软件的数据捕获解决方案；使用 O'Neil 软件和定制软件（使用 Alfresco ECM 和 Hyland OnBase）的记录管理解决方案；以及使用 Liferay 和 MS SharePoint 的门户和协作。

## 关于审阅者

**Ruben Oliva Ramos** 是 Tecnologico de Leon Institute 的计算机系统工程师，拥有萨尔瓦多巴霍大学 Leon 分校计算机与电子系统工程硕士学位，专业为电信信息和网络。他拥有超过 5 年的开发用于控制和监控与 Arduino 和 Raspberry Pi 相连设备的 Web 应用的经验，使用 Web 框架和云服务构建物联网应用。

他是萨尔瓦多巴霍大学的机电一体化教师，教授机电一体化系统设计与工程硕士课程的学生。Ruben 还在墨西哥 Leon 的 Centro de Bachillerato Tecnologico Industrial 225 工作，教授机电一体化技术专业的电子、机器人与控制、自动化和微控制器等课程；他是监控系统和数据记录器数据项目的顾问和开发者，使用技术包括 Android、iOS、Windows Phone、HTML5、PHP、CSS、Ajax、JavaScript、Angular 和 ASP.NET，数据库包括 SQLite、MongoDB 和 MySQL，Web 服务器包括 Node.js 和 IIS，硬件编程包括 Arduino、Raspberry Pi、Ethernet Shield、GPS 和 GSM/GPRS、ESP8266，以及用于数据采集和编程的控制和监控系统。

他撰写了 Packt Publishing 出版的书籍 *Internet of Things Programming with JavaScript*。他还参与使用 Arduino 和 Visual Basic .NET 为 Alfaomega 进行监控、控制和数据采集。

*我谨向我的救世主和主耶稣基督致谢，感谢祂给予我追求此项目的力量和勇气；我最亲爱的妻子 Mayte；我们两个可爱的儿子 Ruben 和 Dario；我亲爱的父亲 Ruben；我最亲爱的母亲 Rosalia；我的兄弟 Juan Tomas；以及我深爱的姐姐 Rosalia，感谢你们在审阅本书期间给予我的所有支持，让我得以追求我的梦想，并在我忙碌的工作日后容忍我不在你们身边。*

**Juan Tomas Oliva Ramos** 是瓜纳华托大学的环境工程师，拥有行政工程与质量硕士学位。他拥有超过 5 年在专利管理与开发、技术创新项目以及通过过程统计控制开发技术解决方案的经验。他自 2011 年起担任统计学、创业和项目技术开发的教师。他一直对通过技术改进和创新流程感兴趣。他成为创业导师和技术管理顾问，并在 Instituto Tecnologico Superior de Purisima del Rincon 创立了技术管理与创业新部门。

他参与了书籍 *Wearable Designs for Smart Watches, Smart TVs and Android Mobile Devices* 的工作。

他通过编程和自动化技术开发了用于改进运营的原型，已申请专利。

> *我想感谢上帝赐予我审阅本书的智慧和谦卑。我想感谢 Ruben 邀请我参与这场冒险。我还要感谢我的妻子 Brenda，我们两个神奇的公主 Regina 和 Renata，以及我们即将到来的成员 Tadeo。你们都是我的力量和幸福，我的愿望是为你们做到最好。*

## www.PacktPub.com

有关您本书的支持文件和下载，请访问 [www.PacktPub.com](www.PacktPub.com)。您知道 Packt 为每本出版的书籍提供电子书版本，可用 PDF 和 ePub 文件吗？您可以在 [www.PacktPub.com](www.PacktPub.com) 升级到电子书版本，并且作为印刷书籍客户，您有权享受电子书副本的折扣。

如需更多详情，请联系我们 `service@packtpub.com`。在 [www.PacktPub.com](www.PacktPub.com)，您还可以阅读大量免费技术文章，订阅一系列免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/9337ac94537bf7e092e1ea6531dce8fd_10_0.png)

[https://www.packtpub.com/mapt](https://www.packtpub.com/mapt)

使用 Mapt 获得最受欢迎的软件技能。Mapt 可让您完全访问所有 Packt 书籍和视频课程，以及业界领先的工具，帮助您规划个人发展并推进职业生涯。
