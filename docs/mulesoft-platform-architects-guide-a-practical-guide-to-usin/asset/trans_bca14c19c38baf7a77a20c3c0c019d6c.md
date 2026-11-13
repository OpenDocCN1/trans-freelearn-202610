
![](image/Cover.png)

# MuleSoft 平台架构师指南

*一本利用 Anypoint Platform 功能进行架构设计、交付和运营 API 的实用指南*

**Jitendra Bafna**  
**Jim Andrews**

![](image/Packt_Logo_New.png)

## 版权

版权所有 © 2024 Packt Publishing

*保留所有权利*。未经出版商事先书面许可，本书的任何部分不得以任何形式或任何方式复制、存储于检索系统或传播，除非是在评论文章或书评中嵌入简短引用的情况。

在本书的编写过程中，已尽一切努力确保所呈现信息的准确性。然而，本书所含信息不提供任何明示或暗示的担保。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接导致的任何损害承担责任。

Packt Publishing 已尽力通过适当使用大写字母，提供书中提及的所有公司和产品的商标信息。然而，Packt Publishing 无法保证该信息的准确性。

**产品群经理**：Aaron Tanna  
**出版产品经理**：Uzma Sheerin  
**高级编辑**：Nisha Cleetus  
**技术编辑**：Vidhisha Patidar  
**责任编辑**：Safis Editing  
**语言支持编辑**：Safis Editing  
**项目协调员**：K Thoshith Vignesh  
**校对员**：Nisha Cleetus  
**索引员**：Hemangini Bari  
**制作设计师**：Prafulla Nikalje  
**市场协调员**：Deepak Kumar 和 Mayank Singh

首次出版：2024 年 7 月  
生产参考：1190724

由 Packt Publishing Ltd. 出版  
Grosvenor House  
11 St Paul's Square  
Birmingham  
B3 1RB, UK

ISBN 978-1-80512-618-8  
[www.packtpub.com](http://www.packtpub.com)

## 献词

本书筹备已久，我要感谢我的父母，他们抚养了我，在我心中播下了知识的种子并悉心培育。感谢我深爱的妻子，你始终是我深刻的灵感源泉和坚定的支持者。感谢我的孩子们，你们是我的命脉和生命之光。

– Jitendra Bafna

献给我的妻子 Debra，你是我持续不断的鼓励、支持和爱。有你和你的微笑在，世界变得有趣得多。献给我的女儿们 Elizabeth 和 Grace，她们已经在世界上化身超级英雄，激励我成为更好的自己，并时刻提醒我要“保持卓越”。献给我的父母和兄弟，感谢你们对人生的指引。还有我的挚友 Gil，感谢他教会我设计的精妙艺术以及良好架构的科学。

– Jim Andrews

## 贡献者

## 关于作者

### Jitendra Bafna

**Jitendra Bafna** 是一位资深解决方案架构师和专家，拥有设计和规划集成及 API 解决方案的丰富经验。他持有 TOGAF 9.2 Level 1 和 Level 2 认证，精通各种集成平台。Jitendra 在架构设计和搭建 MuleSoft 平台方面拥有专长，包括 CloudHub、CloudHub 2.0、Runtime Fabric、混合部署、Flex Gateway 和客户托管平台。他毕业于孟买大学，获计算机科学工程学士学位，并于 2023 年在印度印多尔管理学院 (IIM-I) 完成了数字化转型与创新证书课程。

我首先要感谢我的父母、挚爱的妻子和孩子们，在漫长的写作过程中，他们给予了我持续的支持、耐心和鼓励。感谢所有 MuleSoft 社区管理者、领导者和成员，感谢我从 MuleSoft 社区获得的持续支持、灵感和鼓励。

### Jim Andrews

**Jim Andrews** 是一位集成架构专家、MuleSoft 布道师和终身学习者。在不断演变的技术环境中，他为数十位客户设计和构建集成解决方案已超过 30 年。作为架构方法论的学习者，他持有 TOGAF 9 Level 1 和 Level 2 认证。作为 MuleSoft 认证团队的志愿者，Jim 协助开发了 MuleSoft Developer L2、Integration Associate、Integration and Platform Architecture 的考试。他是 MuleSoft 开发者社区成员和休斯顿 Meetup 组织者，曾在 Meetups、Dreamforce 和 Salesforce TDX 上发表演讲。业余时间，他共同主持播客 *Bits That Bind*。Jim 拥有 Tarleton State University 计算机信息系统理学学士学位。

我要感谢我的妻子，她通过攻读 MBA 的所有写作任务为我树立了榜样。我也要感谢我的合著者 Jitendra，当生活开始给我出难题时，他挺身相助。感谢 Prajakta，每周你都能找到方法鼓励我，让写作变得更轻松。感谢 Mariana Lemus 多年前鼓励我在 MuleSoft 社区中挺身而出，以及其他社区管理者 Sabrina Hockett、Sabrina Marechal、Isabella Navarro——我真心认为你们运营着世界上最好的开发者社区。感谢 Packt 相信我和 Jitendra，给予我们空间、时间、耐心和资源完成这本书。

## 关于审阅者

### Jose Ramon Huerga

**Jose Ramon Huerga** 14 岁时在 80 年代开始了自学编程的旅程，精通 C++。他获得马德里自治大学计算机工程学位，曾在英国、德国、瑞士、阿根廷和西班牙工作，领导过内容管理、集成和 API 管理项目。Jose 是 MuleSoft 大使，领导巴塞罗那 MuleSoft Meetup 小组。他合著了《Kong: Becoming a King of API Gateways》一书，并经常就 API 和集成发表演讲。他与 Margarita 结婚，育有两子 Carlos 和 Victor，为此深感自豪。Jose 喜欢学习新技术并保持更新。

### D. Rajesh Kumar

**D. Rajesh Kumar** 是一位企业架构师，在 IT 行业拥有超过 18 年的丰富经验。过去 10 多年来，他专注于 MuleSoft 平台，跨多个领域从事端到端平台搭建、架构、设计和**赋能中心 (C4E)** 搭建工作。他撰写了多篇技术博客，并担任演讲者和 Meetup 组织者，分享关于高级 MuleSoft 主题、集成策略和架构模式的见解。他是认证的 MuleSoft 架构师和 MuleSoft 大使，因其专业知识和对社区的贡献而受到 MuleSoft 认可。目前，他在 TCS 担任企业架构师，推动大规模数字化集成计划。

## 前言

我们正处于数字化转型时代，组织依靠 API 在企业内部及跨业务合作伙伴之间实现创新。一个强大、安全且灵活的企业级平台是推动成功业务成果的关键，使这种创新得以蓬勃发展。

本书是一本全面的指南，深入探讨 Anypoint Platform 的功能与架构。从概览开始，我们将了解如何利用 MuleSoft 开箱即用的平台功能来满足业务成果和功能需求。随后，本书将引导您了解最佳实践及核心组件的使用，包括 API Manager、Anypoint Monitoring，以及 CloudHub、CloudHub 2.0、Runtime Fabric Manager 和客户托管等托管选项。本书还将探讨如何从项目启动到上线和支持，成功利用 Catalyst 和 Accelerators，实现更快、更高效的开发周期，以便您发布 API 并开始与业务协同创新。

读完本书后，您将能够掌握 Anypoint Platform 的功能，设计出不仅能够支持而且能够赋能贵公司数字化转型的解决方案。

## 本书适合谁
