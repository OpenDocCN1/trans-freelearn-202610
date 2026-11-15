
![](assets/b5f58d72-0138-4193-ab9e-a8684937a516.png)

![](assets/b5f58d72-0138-4193-ab9e-a8684937a516.png)

# 学习 Azure Cosmos DB

## 使用 Cosmos DB 创建可扩展、全球分布且高响应应用程序的初学者指南

### Shahid Shaikh

![](assets/2844f8a2-9710-4a84-beec-bc949706c999.png)

**伯明翰 - 孟买**

---

版权所有 © 2018 Packt Publishing

保留所有权利。未经出版商事先书面许可，本书任何部分不得以任何形式或任何方式复制、存储于检索系统或传播，除非是在评论文章或书评中嵌入简短引用的情况。

本书在编写过程中已竭尽全力确保所呈现信息的准确性。然而，本书所含信息不提供任何明示或暗示的担保。作者、Packt Publishing 及其经销商和分销商均不对本书直接或间接造成的任何损害承担责任。

Packt Publishing 已致力于通过适当使用大写字母，提供书中提及的所有公司和产品的商标信息。然而，Packt Publishing 无法保证该信息的准确性。

**策划编辑：** Sunith Shetty  
**采购编辑：** Chandan Kumar  
**内容开发编辑：** Amrita Noronha  
**技术编辑：** Nilesh Sawakhande  
**责任编辑：** Safis Editing  
**项目协调员：** Shweta H Birwatkar  
**校对员：** Safis Editing  
**索引员：** Aishwarya Gangawane  
**图形设计：** Tania Dutta  
**制作协调员：** Aparna Bhagat

首次出版：2018 年 5 月  
制作参考：1290518

由 Packt Publishing Ltd. 出版  
Livery Place  
35 Livery Street  
Birmingham  
B3 2PB, UK.

ISBN 978-1-78847-617-1

[www.packtpub.com](http://www.packtpub.com)

![](assets/99c429e5-da59-4ca4-8c96-48c4d192cc1f.jpg)[mapt.io](https://mapt.io/)

Mapt 是一个在线数字图书馆，让您完全访问超过 5,000 本书籍和视频，以及行业领先的工具，帮助您规划个人发展并提升职业生涯。欲了解更多信息，请访问我们的网站。

## 为什么要订阅？

*   花更少时间学习，更多时间编码，通过来自 4,000 多位行业专业人士的实用电子书和视频
*   通过专为您打造的技能计划改善学习
*   每月免费获取一本电子书或视频
*   Mapt 支持全文搜索
*   复制粘贴、打印和书签内容

## PacktPub.com

您知道 Packt 为每本出版的书籍提供电子书版本吗？提供 PDF 和 ePub 文件。您可以在 [www.PacktPub.com](http://www.PacktPub.com) 升级到电子书版本，作为纸质书客户，您有权享受电子书副本的折扣。如需更多详情，请通过 `service@packtpub.com` 联系我们。

在 [www.PacktPub.com](http://www.PacktPub.com)，您还可以阅读免费技术文章合集，注册各类免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

## 贡献者

## 作者简介

**Shahid Shaikh** 是一名工程师、出版作家和博主。他在自己的博客上撰写了大量关于 Node.js 和各种数据库的教程。他信奉开源，几乎所有源代码都在 GitHub 上。他还开发并推出了一款名为 Spark 的 SaaS 产品，用于处理内容分享自动化。目前，他正在构建一款名为 Hashmailer 的下一代电子邮件营销工具。

> *我要感谢我的父母在写书阶段一直支持我。我不得不利用周末工作，取消许多计划以按时交付章节，我的家人和朋友因我取消了各种安排，并在写书期间支持我，对此我深表感激。*

## 审阅者简介

**Kasam Ahmed Shaikh** 是一位资深专业人士，拥有“could be”态度，在印度孟买一家领先的 IT 公司拥有 10 年坚定的行业经验。他是一名认证 Azure 架构师，被认可为 C# Corner MVP。他也是一位演讲者，并撰写过一本关于 Azure 认知服务的书。他还是 DearAzure.net 和 Shaikh Academy 的创始人。

> *首先，我要感谢真主安拉、我的家人，特别是我的另一半，在整个过程中激励我。我非常感谢 Packt Publishing 相信并考虑我获得这个机会。*

## Packt 正在寻找像您这样的作者

如果您有兴趣成为 Packt 的作者，请访问 [authors.packtpub.com](http://authors.packtpub.com) 并立即申请。我们已与数千名开发人员和技术专业人士合作，就像您一样，帮助他们与全球技术社区分享见解。您可以提交一般申请，申请我们正在招募作者的特定热门主题，或提交您自己的想法。

## 目录

1.  [标题页](`3c82ef13-572f-4d7d-8018-1c7870dd639f.xhtml`)
2.  [版权和致谢](`edcb7632-2865-4ec1-acf7-d1a13e207809.xhtml`)
    1.  [Learning Azure Cosmos DB](`913f4c12-7d42-4d8d-937a-7512237f0d35.xhtml`)
3.  [Packt 推广](`af31b4d9-dd3f-4fd2-9d0a-04b98632b7bc.xhtml`)
    1.  [为什么要订阅？](`6a107e52-9d8b-4f60-937e-de668a7379d2.xhtml`)
    2.  [PacktPub.com](`90547174-f4b9-462e-951f-4336b8bccbe7.xhtml`)
4.  [贡献者](`23bcf460-6981-4e0a-a5cb-99368397cd3f.xhtml`)
    1.  [作者简介](`9f275e7f-df46-4545-964c-9d6e63c6f1b8.xhtml`)
    2.  [审阅者简介](`4488d331-82c4-4841-9f2b-c1fa6a32f0c5.xhtml`)
    3.  [Packt 正在寻找像您这样的作者](`b94ba860-91ab-44ba-bf00-45bf135544f6.xhtml`)
5.  [前言](`6b5e934b-1812-42be-b262-69fe069c8269.xhtml`)
    1.  [适读人群](`9b7864a3-b299-4401-a71a-0168cc7ee313.xhtml`)
    2.  [本书内容](`a22c283c-f6ef-45a9-8e67-11033cca0953.xhtml`)
    3.  [如何充分利用本书](`6e40524b-825e-49aa-a6f3-f72dd44399cf.xhtml`)
        1.  [下载示例代码文件](`b94e21bd-cfdc-452d-a403-4624ef0da98d.xhtml`)
        2.  [下载彩色图片](`6bf7bd2b-0c6a-4e16-a5d1-d789b2e29762.xhtml`)
        3.  [约定](`cf50cf66-d64a-4d89-87dd-be3a2ed9a2c0.xhtml`)
    4.  [联系我们](`72f1c448-8d78-4922-a261-75078e747e54.xhtml`)
        1.  [评论](`5e87a91f-0672-4cea-a095-27a5ea04a95b.xhtml`)
6.  [Microsoft Azure 云和 Cosmos DB 简介](`25826e74-4071-4c8f-af67-9f5185cd8e67.xhtml`)
    1.  [Microsoft Azure 云功能](`43ab6d8c-96b8-4fb2-85f7-43d3ffdb1579.xhtml`)
    2.  [Microsoft Azure 作为 SPI](`7eab4a4b-6cf0-4907-822a-f46ce8a0a555.xhtml`)
        1.  [Azure 作为 PaaS](`b87b31f9-ac41-443a-b881-68d8411de11c.xhtml`)
        2.  [Azure 作为 IaaS](`91f2e807-ca8b-4f09-8dce-c4479cdab6e6.xhtml`)
    3.  [Azure 组件](`f0437b2b-f47e-40e5-b8be-363626096dae.xhtml`)
    4.  [Azure 入门](`b05277bb-f949-4ffd-bc9a-5b4db80cbcdf.xhtml`)
        1.  [计算模块](`3c5fdeff-255c-404f-ba36-20840896d753.xhtml`)
        2.  [数据管理模块](`fd6a4a89-ebb3-4ee8-b5cf-6c3fa447a30a.xhtml`)
        3.  [网络模块](`4bab3f94-5339-4f86-b4cf-8342fbda6c11.xhtml`)
        4.  [大数据模块](`26ae0a8f-2de6-4095-9d97-b876ec842ad2.xhtml`)
        5.  [缓存模块](`f517d6d6-8843-4748-ae9e-eb3bcfa11bc2.xhtml`)
        6.  [消息传递模块](`17f39f6f-f6da-437a-8150-48da4314afe3.xhtml`)
        7.  [身份模块](`e2ff2e10-cf1d-4eff-a449-db376876040f.xhtml`)
        8.  [移动服务模块](`18bc91fa-c11f-427b-bb82-a8b642b4a8bc.xhtml`)
        9.  [IoT 模块](`af4df387-5f86-41f9-bd99-2eb4ba715c9a.xhtml`)
    5.  [Azure Cosmos DB](`bef4c1ef-9233-4c71-bb8f-b7cdb8e208c7.xhtml`)
        1.  [交钥匙全球分发](`fd7c157f-10ca-4191-b659-1e0f41addc63.xhtml`)

