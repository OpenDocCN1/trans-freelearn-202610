# MuleSoft 平台架构师指南：Anypoint Platform 能力使用指南——架构设计、交付与运营 API

# 献词

本书筹备已久，我要感谢我的父母，他们抚养了我，在我心中播下了知识的种子并悉心培育。感谢我深爱的妻子，你始终是我深刻的灵感源泉和坚定的支持者。感谢我的孩子们，你们是我的命脉和生命之光。

– Jitendra Bafna

献给我的妻子 Debra，你是我持续不断的鼓励、支持和爱。有你和你的微笑在，世界变得有趣得多。献给我的女儿们 Elizabeth 和 Grace，她们已经在世界上化身超级英雄，激励我成为更好的自己，并时刻提醒我要“保持卓越”。献给我的父母和兄弟，感谢你们对人生的指引。还有我的挚友 Gil，感谢他教会我设计的精妙艺术以及良好架构的科学。

– Jim Andrews

# 贡献者

# 关于作者

## Jitendra Bafna

**Jitendra Bafna** 是一位资深解决方案架构师和专家，拥有设计和规划集成及 API 解决方案的丰富经验。他持有 TOGAF 9.2 Level 1 和 Level 2 认证，精通各种集成平台。Jitendra 在架构设计和搭建 MuleSoft 平台方面拥有专长，包括 CloudHub、CloudHub 2.0、Runtime Fabric、混合部署、Flex Gateway 和客户托管平台。他毕业于孟买大学，获计算机科学工程学士学位，并于 2023 年在印度印多尔管理学院 (IIM-I) 完成了数字化转型与创新证书课程。

我首先要感谢我的父母、挚爱的妻子和孩子们，在漫长的写作过程中，他们给予了我持续的支持、耐心和鼓励。感谢所有 MuleSoft 社区管理者、领导者和成员，感谢我从 MuleSoft 社区获得的持续支持、灵感和鼓励。

## Jim Andrews

**Jim Andrews** 是一位集成架构专家、MuleSoft 布道师和终身学习者。在不断演变的技术环境中，他为数十位客户设计和构建集成解决方案已超过 30 年。作为架构方法论的学习者，他持有 TOGAF 9 Level 1 和 Level 2 认证。作为 MuleSoft 认证团队的志愿者，Jim 协助开发了 MuleSoft Developer L2、Integration Associate、Integration and Platform Architecture 的考试。他是 MuleSoft 开发者社区成员和休斯顿 Meetup 组织者，曾在 Meetups、Dreamforce 和 Salesforce TDX 上发表演讲。业余时间，他共同主持播客 *Bits That Bind*。Jim 拥有 Tarleton State University 计算机信息系统理学学士学位。

我要感谢我的妻子，她通过攻读 MBA 的所有写作任务为我树立了榜样。我也要感谢我的合著者 Jitendra，当生活开始给我出难题时，他挺身相助。感谢 Prajakta，每周你都能找到方法鼓励我，让写作变得更轻松。感谢 Mariana Lemus 多年前鼓励我在 MuleSoft 社区中挺身而出，以及其他社区管理者 Sabrina Hockett、Sabrina Marechal、Isabella Navarro——我真心认为你们运营着世界上最好的开发者社区。感谢 Packt 相信我和 Jitendra，给予我们空间、时间、耐心和资源完成这本书。

# 关于审阅者

## Jose Ramon Huerga

**Jose Ramon Huerga** 14 岁时在 80 年代开始了自学编程的旅程，精通 C++。他获得马德里自治大学计算机工程学位，曾在英国、德国、瑞士、阿根廷和西班牙工作，领导过内容管理、集成和 API 管理项目。Jose 是 MuleSoft 大使，领导巴塞罗那 MuleSoft Meetup 小组。他合著了《Kong: Becoming a King of API Gateways》一书，并经常就 API 和集成发表演讲。他与 Margarita 结婚，育有两子 Carlos 和 Victor，为此深感自豪。Jose 喜欢学习新技术并保持更新。

## D. Rajesh Kumar

**D. Rajesh Kumar** 是一位企业架构师，在 IT 行业拥有超过 18 年的丰富经验。过去 10 多年来，他专注于 MuleSoft 平台，跨多个领域从事端到端平台搭建、架构、设计和**赋能中心 (C4E)** 搭建工作。他撰写了多篇技术博客，并担任演讲者和 Meetup 组织者，分享关于高级 MuleSoft 主题、集成策略和架构模式的见解。他是认证的 MuleSoft 架构师和 MuleSoft 大使，因其专业知识和对社区的贡献而受到 MuleSoft 认可。目前，他在 TCS 担任企业架构师，推动大规模数字化集成计划。

# 前言

我们正处于数字化转型时代，组织依靠 API 在企业内部及跨业务合作伙伴之间实现创新。一个强大、安全且灵活的企业级平台是推动成功业务成果的关键，使这种创新得以蓬勃发展。

本书是一本全面的指南，深入探讨 Anypoint Platform 的功能与架构。从概览开始，我们将了解如何利用 MuleSoft 开箱即用的平台功能来满足业务成果和功能需求。随后，本书将引导您了解最佳实践及核心组件的使用，包括 API Manager、Anypoint Monitoring，以及 CloudHub、CloudHub 2.0、Runtime Fabric Manager 和客户托管等托管选项。本书还将探讨如何从项目启动到上线和支持，成功利用 Catalyst 和 Accelerators，实现更快、更高效的开发周期，以便您发布 API 并开始与业务协同创新。

读完本书后，您将能够掌握 Anypoint Platform 的功能，设计出不仅能够支持而且能够赋能贵公司数字化转型的解决方案。

# 本书适合谁



## 目标读者

- 具备集成与 API 知识的技术架构师，希望了解如何使用 MuleSoft Anypoint Platform 实施这些解决方案
- 想要转型为 MuleSoft 平台架构师、计划参加 MuleSoft 平台架构师认证考试并希望深入了解平台能力的 MuleSoft 高级开发人员
- 需要了解并定义 MuleSoft 托管选项、负责组织 Anypoint Platform 战略的基础设施架构师


# 充分利用本书

| 本书涵盖的软件/硬件 | 操作系统要求 |
| --- | --- |
| Java 8 或 Java 17 | Windows、macOS 或 Linux |
| Maven | |
| Anypoint Studio IDE 和 Anypoint Platform | |

# 下载彩色图片

我们还提供了一个 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。您可以在此处下载：[`packt.link/gbp/9781805126188`](https://packt.link/gbp/9781805126188)。

## 图片免责声明

本书中的某些图片仅用于提供上下文，图形的可读性对讨论并非至关重要。请参考我们的免费图形包以下载图片。您可以从 [`packt.link/gbp/9781805126188`](https://packt.link/gbp/9781805126188) 下载图片。

# 约定用法

全书使用了多种文本约定。

## 文本中的代码

表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如："备份和恢复过程通过 `rtfctl` 很简单。"

## 代码块

代码块设置如下：

```
<plugin>
  <groupId>org.mule.tools.maven</groupId>
  <artifactId>mule-maven-plugin</artifactId>
  <version>3.8.6</version>
  <extensions>true</extensions>
  <configuration>
    <standaloneDeployment>
      <muleHome>${mule.home }</muleHome>
      <muleVersion>${app.runtime}</muleVersion>
    </standaloneDeployment>
  </configuration>
</plugin>
```

## 命令行输入/输出

任何命令行输入或输出均按如下方式编写：

```
./rtfctl backup <path_to_store_backup>
```

## 粗体

表示新术语、重要单词或您在屏幕上看到的单词。例如，菜单或对话框中的单词以**粗体**显示。例如："专门检查**设计架构与实现**，如*图 3.1*所示，我们发现了一个可能有用的剧本步骤。"

## 提示或重要说明

显示如下。

# 联系我们

我们始终欢迎读者的反馈。

## 一般反馈

如果您对本书的任何方面有疑问，请发送邮件至 customercare@packtpub.com，并在邮件主题中注明书名。

## 勘误

虽然我们已尽一切努力确保内容的准确性，但错误难免发生。如果您在本书中发现错误，我们将不胜感激如果您能向我们报告。请访问 [www.packtpub.com/support/errata](http://www.packtpub.com/support/errata) 并填写表格。

## 盗版

如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供位置地址或网站名称。请通过 copyright@packt.com 联系我们，并附上相关材料的链接。

## 如果您有兴趣成为作者

如果您在某个主题上有专长，并有兴趣撰写或为书籍做贡献，请访问 [authors.packtpub.com](http://authors.packtpub.com)。

# 分享您的想法

读完 *MuleSoft Platform Architect's Guide* 后，我们非常希望听到您的想法！请[点击此处直接前往亚马逊评论页面](https://packt.link/r/1805126180)为本书留下反馈。

您的评论对我们和技术社区都很重要，将帮助我们确保提供优质内容。

# 下载本书的免费 PDF 副本

感谢您购买本书！

您是否喜欢随时随地阅读，却无法随身携带纸质书？

您购买的电子书是否与您选择的设备不兼容？

别担心，现在购买每本 Packt 书籍，您都可以免费获得该书的无 DRM 保护的 PDF 版本。

随时随地，在任何设备上阅读。直接从您喜爱的技术书籍中搜索、复制并粘贴代码到您的应用程序中。

福利不止于此，您还可以获得独家折扣、新闻通讯和每天发送到您收件箱的精彩免费内容。

请遵循以下简单步骤获取福利：

1. 扫描二维码或访问下方链接

![二维码](img/B19922_QR_Free_PDF.jpg)

[`packt.link/free-ebook/9781805126188`](https://packt.link/free-ebook/9781805126188)

2. 提交购买证明

3. 就这些！我们会将免费 PDF 和其他福利直接发送到您的邮箱。
