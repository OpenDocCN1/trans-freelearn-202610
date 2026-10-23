## 制作人员

| 角色 | 姓名 |
| --- | --- |
| 作者 | William J. Carpenter |
| 编辑团队负责人 | Gagandeep Singh |
| 审阅人 | Philip Hirschi, Raffaele Manfellotto, Ian Story |
| 项目团队负责人 | Ashwin Shetty |
| 项目协调员 | Zainab Bagasrawala |
| 选题编辑 | Amey Kanse |
| 校对员 | Aaron Nash |
| 开发编辑 | Susmita Panda |
| 美术设计 | Geetanjali Sawant |
| 技术编辑 | Arani Roy |
| 制作协调员 | Arvindkumar Gupta, Alwin Roy |
| 索引编制人 | Monica Ajmera Mehta, Rekha Nair |
| 封面制作 | Arvindkumar Gupta |

## 作者简介

**William J. Carpenter** 是 IBM 位于西雅图地区的 ECM 架构师。Bill 自 1998 年起从事企业内容管理业务，拥有开发者、开发经理和架构师的经验。他是 *IBM FileNet Content Manager 实施最佳实践与建议* 和 *使用 IBM FileNet P8 API 开发应用程序* 的合著者，是 IBM developerWorks 的特约作者，并且经常在会议上发表演讲。他在财富50强公司拥有构建大型软件系统的经验，还曾担任一家互联网初创公司的首席技术官。他经常为多个开源项目贡献邮件列表和补丁。Bill 拥有纽约州特洛伊市伦斯勒理工学院的数学和计算机科学学位。

## 致谢

如果没有多年来在学习 ECM 知识的过程中获得的无数直接和间接帮助，我不可能写成这本书。这些帮助主要来自两个方面。首先，IBM 的众多同事（以及 IBM 之前 FileNet 时期的同事）教会了我关于 ECM 的大部分知识（在此之前，我甚至不会拼写 ECM，而现在……）。感谢那些在产品开发、技术支持、现场实施中工作的同事，以及这些年来我认识的其他同事；尤其是，我感谢我的架构师同事们，我们就无数话题进行了耐心、专业且详细的讨论。其次，我从我们的客户、合作伙伴以及与客户最紧密合作的 IBM 同事那里学到了很多关于 ECM 的知识。尽管 P8 是一个平台，但我们的大部分工作都是由用例驱动的。听到新的用例总是令人愉快且富有教育意义（即使讨论的内容是我们没有的功能）。

我特别感谢技术审阅人。他们花费大量时间阅读本书所有章节并提出意见，从他们发给我的大量评论和建议中可见一斑。本书因此大为改进。也感谢 Packt Publishing 的编辑和工作人员，他们给了我写作的机会，并指导我走向出版之路。当然，最终文本中任何遗留的错误都由我全权负责。

最后，也是最重要的，我要感谢我的妻子、女儿和儿子，感谢他们在过去几个月里耐心忍受我对他们的冷落。尽管我缺席了许多家务活动、家庭项目和职责，他们仍不断鼓励我写作这本书。（我知道你在想什么，但我不会感谢家里的猫。事实证明，他对写作部分帮不上什么忙。而且，他的技术想法简直是一派胡言。）

—— WJC，西雅图，2010年12月

## 审阅人简介

**Ian Story** 自 1998 年起从事企业内容管理领域的工作，担任过多种技术与管理工作角色，曾就职于 Weyerhaeuser、Washington Mutual 和 JPMorgan Chase & Co. 等公司。他拥有太平洋路德大学的计算机科学和商业本科学位，现居住于华盛顿州西雅图郊区。他在 ECM 社区非常活跃，担任太平洋西北 IBM ECM UserNet 总裁、美洲 IBM ECM 委员会成员、IBM ECM 技术咨询委员会成员，以及 IBM 信息大使。Ian 还是 Bucket2 的联合创始人，这是一家专注于移动应用的私有软件研发公司；可通过 Bucket2 网站 http://www.bucket2.com 与他联系。

我要感谢作者 Bill Carpenter 给我机会审阅本书并提供评论，同时感谢 ECM 生态系统中的每一个人——客户、合作伙伴和 IBM 本身——多年来的友谊。

## www.PacktPub.com

### 支持文件、电子书、折扣优惠及其他

您可能希望访问 www.PacktPub.com 获取与您的书相关的支持文件和下载。

您是否知道 Packt 为每本出版的书提供电子书版本，包含 PDF 和 ePub 文件？您可以在 www.PacktPub.com 升级到电子书版本，作为印刷书客户，您有权享受电子书折扣。如需更多详情，请通过 service@packtpub.com 与我们联系。

在 www.PacktPub.com 上，您还可以阅读一系列免费技术文章，订阅多种免费通讯，并享受 Packt 图书和电子书的独家折扣与优惠。

![](img/2295191c450a0c86b2d82df6cec04526_8_0.png)

http://PacktLib.PacktPub.com

您是否需要即时解决 IT 问题？PacktLib 是 Packt 的在线数字图书库。在这里，您可以访问、阅读和搜索 Packt 的整个图书库。

### 为什么要订阅？

- 可全文搜索 Packt 出版的所有图书
- 可复制粘贴、打印和收藏内容
- 按需提供，可通过网页浏览器访问

### Packt 账户持有人的免费访问

如果您在 www.PacktPub.com 拥有 Packt 账户，即可使用该账户立即访问 PacktLib，查看九本完全免费的图书。只需使用您的登录凭据即可立即访问。

### 新书即时更新

获取通知！关注 Twitter 上的 @PacktEnterprise 或 Packt Enterprise Facebook 页面，即可了解新书发布时间。




Scripts and desktop shortcuts 64
Summary 67
## Chapter 3: Installing the Content Engine 69
- An aside about some names 69
- P8 platform architecture 70
  - Content Engine (CE) 71
  - Other components 73
    - Application Engine (AE) 73
    - Content Search Engine (CSE) 74
    - Process Engine (PE) 74
    - Rendition Engine (RE) 74
- Installing the CE 75
  - Getting the software 75
  - Running the CE server installer 76
  - Configuration Manager 76
  - WAS console tweaks 81
  - Installing FEM 83
  - Bringing up the P8 Domain 88
  - Create an ObjectStore 90
- Summary 92
## Chapter 4: Administrative Tools and Tasks 93
- Domain and GCD 94
  - Topology levels 98
  - Using topology levels 100
- Exploring Domain-level items 100
  - AddOns 102
  - Fixed Content Devices 103
  - Server Cache configuration 103
  - Content configuration 105
  - Trace logging 106
  - Content cache 109
- Exploring Object Store-level Items 109
  - Content Access Recording Level 110
  - Auditing 111
  - Checkout type 111
  - Text Index Date Partitioning 112
  - Cache configuration 113
- Metadata 114
  - Property templates 114
  - Choice lists 115
  - Classes 116
  - Subclassing example 117
Object Browse and Query 124
Summary 126
## Chapter 5: Installing Other Components 127
- Content Search Engine (CSE) 129
  - The CSE user 130
  - Running the CSE installer 130
  - Command line configuration steps 133
  - Configuring CSE via the K2 Dashboard 134
  - Configuring CE for CSE via FEM 136
  - Configuring the P8 Domain 137
  - Configuring an Object Store 138
- Process Engine server (PE server) 141
  - Users and groups 142
  - Database configuration 143
    - DB2 database and tablespaces 143
    - DB2 user permissions 144
    - DB2 client software 144
  - PE server installation and configuration 145
    - PE server installation 146
    - CE client installation 146
    - PE server configuration 147
    - Shared memory 149
    - Test the connections 150
    - CE configuration for PE 150
    - PE client software 151
- Application Engine (AE) 151
- Workplace XT (XT) 151
  - Configure LDAP 152
  - Trust relationships and LTPA 154
  - Run the installer 155
  - CE and PE client software 156
    - CE client 156
    - PE client 157
  - XT pre-deployment configuration 157
  - Deploying XT 158
    - Classloader configuration 159
    - Map special subjects 160
    - ORB uniqueness 160
    - Running XT the first time 160
- IBM System Dashboard for ECM (SD) 162
- Rendition Engine (RE) 163
- Summary 164
## Chapter 6: End User Tools and Tasks 165
- What is Workplace XT? 166
  - Browsing folders and documents 167
  - Adding folders 167
  - Adding documents 168
  - Viewing documents 170
- Entry templates 171
- Workflow interactions 172
  - One-time isolated region setup 173
  - Approval workflows 174
  - Tasks in XT 175
- Versioning 176
- Properties and security 179
- Searches 180
  - Simple Search 181
  - Keyword Search 182
  - Advanced Search 182
  - Stored Searches and Search Templates 183
- Summary 185
## Chapter 7: Major CM Features 187
- Documents 188
  - Content 188
    - Multiple content elements 189
    - Content transfer and content reference 189
    - Content element numbering 190
  - Versioning 191
    - Checkout and checkin 192
    - Freeze 193
    - Major and minor versions 194
  - Document lifecycles 195
  - Autoclassification 196
  - Compound documents 197
  - DITA publishing 198
  - Rendition Engine 198
- Search 199
  - Merge mode 200
  - Selectable and searchable properties 201
  - Property searches and full-text searches 201
  - JDBC provider 202
  - Search Templates and Stored Searches 203
- Folders and containment 204
- Referential containment 204
- Filing 205
- Containment names 207
- The decision to file documents 208
- Custom properties and classes 209
  - Properties of Class Definitions 210
  - Properties of Property Definitions 210
- Custom objects 212
- Annotations 213
- Links 214
- Subscriptions, events, and auditing 215
  - What is an event? 216
  - Subscriptions 217
  - Audit logging 219
  - Content Access Recording Level 219
  - Event handlers 220
  - Workflow launch 220
- AddOns 221
  - AddOn components 222
  - Creating and installing AddOns 222
  - Authoring an AddOn 223
- Summary 224
## Chapter 8: Security Features and Planning 225
- Authentication or authorization? 225
- Authentication in CM 226
  - Java Authentication and Authorization Service (JAAS) 227
  - Where authentication actually happens 230
    - Application Server trust relationships 230
    - Thick EJB clients 231
    - CEWS clients 232
  - Single Sign-on 235
  - Anonymous and guest access 236
  - Impersonation and run-as 237
- Enterprise directories 239
- Authorization in CM 240
  - Discretionary and Mandatory Access Control 240
  - Access Control Lists 241
    - User and group access 243
    - Rights 243
    - Levels 244
    - Unused bits 245
  - Implicit rights 245
- Object Owner 246
- Object Store administrator 246
- Special query right 247
- Extra access requirements 247
  - Modification Access Required 247
  - Target Access Required 248
- Default instance security 249
- #CREATOR-OWNER 250
- Security policy and document lifecycle policy 250
  - Security Policy and Security Templates 251
  - Document lifecycle policy 252
- Dynamic security inheritance 252
  - Inheritable depth 253
  - Parent and child security objects 253
  - System-defined inheritance 255
  - Roles and adapters 256
  - Project team example 256
  - Project team adapters 257
  - Folders as adapters 259
- Marking sets 259
  - A marking set example 261
- The final veto 262
- A hypothetical scenario 263
  - The players 263
  - The business requirements 263
  - The strategy 265
  - The implementation 265
  - The test 267
- Summary 268
## Chapter 9: Planning Your Deployment 269
- Distributed deployments in functional tiers 270
  - Web browsers 270
  - Web servers 271
  - CE, PE, and friends 272
  - Databases and filesystems 272
- Custom applications 273
- Parallel environments 273
  - Pre-production 274
  - Development and testing 274
- How many domains? 275
- How many Object Stores? 277
- Network security 278
  - TLS/SSL 278
  - Firewalls 279



## 第 10 章：包含和附加组件 293
- 标准 CM 组件 294
  - 服务器组件 294
  - 应用和连接器 295
  - 环境组件 296
- 计划和场景 297
- 合规管理 297
- IBM 企业记录（IER） 298
  - 声明 298
  - 分类 299
  - 保护 299
  - 处置 299
  - 审计和报告 301
- IBM eDiscovery 301
  - IBM eDiscovery Analyzer（eDA） 302
  - IBM eDiscovery Manager（eDM） 303
- IBM Content Collector 303
  - ICC for File Systems 304
  - ICC for Microsoft SharePoint 304
  - ICC for Email 304
  - 任务连接器 304
- IBM Classification Module 305
### 智能归档策略 306
  - 摄取 307
  - 基础设施 307
  - 管理 307
### IBM FileNet Business Process Manager 308
  - IBM FileNet Business Process Framework 308
  - IBM Enterprise Content Management Widgets 309
### 其他组件 309
  - IBM FileNet System Monitor 309
  - IBM FileNet Image Services 309
  - IBM FileNet Capture and Datacap 310
  - Content Management Interoperability Services 310
  - Darwin Information Typing Architecture 311
  - IBM Content Analytics 311
### 总结 312
## 第 11 章：应用开发初探 313
### Content Engine API 314
  - 不要绕过 API 314
    - 读取 314
    - 更新 315
  - API 传输 316
    - CEWS 传输 316
    - EJB 传输 317
    - 用户事务 318
    - JAAS 上下文 319
    - 传输特定编码 319
  - CE .NET 和 Java API 320
  - CEWS 协议 321
  - 附件格式 322
  - 兼容层 323
    - Java 兼容层 324
    - COM 兼容层 325
    - CEWS 3.5 协议 325
### 其他 API 326
  - PE API 326
  - CMIS 326
  - ECM Widgets 327
  - AE/XT 自定义和集成 328
### 开发环境 328
  - Eclipse 中的 CE Java API 329
    - 项目 330
    - CE API 依赖 332
  - 应用代码 333
  - 运行应用 335
  - 我们未展示的一些内容 336
  - VS C# Express 中的 CE .NET API 337
    - 项目 337
    - CE API 依赖 339
    - 应用代码 340
    - 运行应用 342
    - 我们未展示的一些内容 342
  - 总结 342
## 第 12 章：DUCK 示例应用 343
- 业务需求 344
  - 受限数量文档 344
  - 受限流通文档 345
- 终端用户视图设计 345
  - 通用登录屏幕 346
  - 查找文档 347
  - 内容下载 348
  - 添加文档 349
  - 查看文档详情 350
  - 受限数量文档 351
  - 受限流通文档 352
  - 修改文档 352
- 企业要求 354
  - 导航概览 354
- 数据模型和安全模型 356
  - 文档属性 356
  - 限制指示器 357
  - 受限流通 357
  - 访问日志 358
  - 访问控制 359
  - 访问请求 359
  - 受限数量 361
- 技术实现细节 363
  - CE 元数据更改 363
  - 注解子类 364
  - 自定义文档属性 364
  - 自定义事件 364
  - 审计日志配置 365
  - 部署事件处理程序代码模块 366
  - DuckRQB 项目 367
    - 新代码模块 368
    - 修改代码模块 368
    - 调试日志 370
  - Duck 项目 371
  - 导出和部署 Duck.war 374
    - 部署描述符 375
    - 部署到 WAS 376
    - 选择 JSF 运行时 376
    - 配置角色映射 377
    - 启动应用 377
### 总结 378
## 第 13 章：支持、修复包和故障排除 379
### 资源 379
  - 文档 380
    - 信息中心 380
    - 独立文档 381
    - 其他链接 382
    - IBM Redbooks 383
    - IBM developerWorks 384
    - 全球 IBM ECM 社区 384
  - 在线支持资源 385
    - 支持门户 385
    - PMR 和 APAR 385
    - 技术说明 385
    - 个性化通知 386
    - Fix Central 386
  - 按需信息大会 387
### 版本和修复 387
  - 产品发布 387
  - 临时修复 389
  - 修复包 391
  - 安装修复包 393
    - 支持组件 393
    - CM 组件 394
    - CE 4.5.1.1.4 服务器 395
    - CSE 4.5.1.1 客户端 396
    - CSE 4.5.1.1 服务器 397
    - 重新部署更新后的 CE 服务器 398
    - CE 4.5.1.4 客户端 401
    - XT 1.1.4.8 402
### 故障排除 404
  - 预防 404
    - 配置控制 404
    - 快照 405
    - 备份 405
  - 寻找问题 406
    - 初始配置 406
  - 身份验证 407
  - WAS 启动信息 409
  - 性能 409
  - 跟踪日志 410
  - 隔离 410
  - 数据库调优 411
### 总结 412
### 索引 413




## 前言

从企业内容管理概述开始，本书迅速进入搭建实际内容管理器系统的实务。你将学习关键的内容管理器应用，并通过演示了解作为开发者、管理员或终端用户所关心的核心概念。书中另有专门章节描述主要平台功能、安全相关功能，以及与其他常用软件组件的集成。一个逼真的示例应用在你面前逐步展开，展现了 IBM FileNet P8 Content Manager 的精妙之处。最后，你将深入了解故障排除、支持站点和在线资源，以帮助满足持续的需求。

## 本书涵盖内容

- 第 1 章，*什么是 ECM？*，提供了企业内容管理（ECM）的含义和历史背景。如果你刚开始接触 ECM，本章将为你提供有趣的背景，以便更好地理解本书其余内容以及即将到来的 ECM 体验。

- 第 2 章，*安装环境组件*，关注我们所称的环境组件的安装，这些是先决条件组件，并非 IBM FileNet Content Manager（CM）本身的一部分。我们将介绍配置环境组件的示例，包括数据库、目录服务器和 J2EE 应用服务器，IBM FileNet 软件依赖于这些组件。

- 第 3 章，*安装内容引擎*，着手实际安装内容引擎（CE）软件。除了 CE 本身之外，你还将安装 FileNet Enterprise Manager 管理工具，并执行初始化域和创建第一个对象存储所需的步骤。到本章结束时，你将拥有一个可工作的最小系统。

- 第 4 章，*管理工具和任务*，简要介绍了 CM 的一些常见管理功能。本章描述了许多管理概念，并使用 FileNet Enterprise Manager 来说明探索和配置其中的大部分功能。

- 第 5 章，*安装其他组件*，通过安装 CE 环境的一部分的几个附加组件，继续安装之旅。特别是，它将涵盖内容搜索引擎的安装。你还将安装流程引擎和 Workplace XT 应用，并探索更多 CM 生态系统。

- 第 6 章，*终端用户工具和任务*，主要通过 Workplace XT，探索普通用户而非管理员常用的操作。它包括浏览、搜索和操作文档等内容。

- 第 7 章，*主要 CM 功能*，探讨了内容管理器（CM）的大多数功能，并讨论了主要组件架构。这些解释是从功能在内容引擎内部的设计工作方式的角度出发的，而非针对任何特定应用。因此，这些材料适用于自定义和开箱即用的应用。

- 第 8 章，*安全功能和规划*，讨论了 CM 安全的诸多方面。CM 全面的安全功能是其主要优势之一。你在其他章节中已零散地看到过安全相关内容。现在是时候对这些内容进行更全面的审视了。

- 第 9 章，*规划你的部署*，从讨论单个功能转向从宏观角度审视如何为 ECM 平台的组件规划并部署到实际环境中。

- 第 10 章，*包含和附加组件*，概述了与基础 CM 平台一起最常用于构建 ECM 解决方案的其他组件。

- 第 11 章，*应用开发初探*，提供了 IBM FileNet Content Manager（CM）应用开发的简要概述。它对 CM 提供的各种 API 进行了高层次概述，并讨论了一些开发方法论和理念。

- 第 12 章，*DUCK 示例应用*，包含一个完整、自包含的示例应用，演示了多个 CM 功能。本章以假设的应用开发案例研究形式撰写，从业务需求开始，一直到最终应用的可工作原型。

- 第 13 章，*支持、修复包和故障排除*，描述了在线及其他资源，帮助你了解产品功能、修复和更新。它还讨论了一些常见的故障排除情况和技术，以及 CM 修复包的应用。

## 阅读本书所需条件

本书对 IBM FileNet Content Manager 4.5.1 的安装和操作进行了广泛讨论。如果你手头有 P8 组件的安装文件，将更能从这些讨论中受益。我们将使用的确切组件细节从第 2 章开始给出。

虽然不必完全匹配 FileNet 及支持软件的版本，但匹配程度越高，需要做的调整就越少。我们的环境对除流程引擎和 FileNet Enterprise Manager 之外的所有组件均使用 Red Hat Enterprise Linux（这两个使用 Windows Server）。我们将使用 Tivoli Directory Server、DB2 Universal Database 和 WebSphere Application Server。在安装和配置软件的早期章节中，使用确切的组件最为重要。一旦软件运行起来，功能特性在所有平台上基本相同。要按照我们的步骤搭建一个用于教育或演示目的的可工作系统，并不需要高端服务器硬件，但如果内存不足，可能会运行缓慢。

你可以在没有 P8 系统的情况下阅读本书受益，但如果你有一个系统可用，以便尝试我们描述的内容并自行实验，将获益更多。该系统可以是你阅读早期章节时安装的系统，也可以是单独搭建的系统。

## 本书适合人群

如果你是 CIO、IT 经理、IT 工程师，或许是业务领导者、业务分析师，甚至是希望更好地理解内容管理器在企业中作用的终端用户，无论是否之前了解过 IBM FileNet P8 Content Manager 或企业内容管理，本书对你来说必不可少。

如果你是刚开始使用内容管理器的人，或正面临是否使用内容管理器或其他解决方案的决策，请放心，你找不到更好的指南。即使你有数年的经验，也能通过清晰、实用的基础和高级功能解释来丰富你的知识。虽然它本身不是开发人员指南，但 API 概述和示例应用将帮助你作为企业开发者或架构师更好地掌握内容管理器技术。

## 约定

在本书中，你会发现多种文本样式，用以区分不同类型的信息。以下是一些示例及其含义。

正文中出现的代码词如下所示："In that case, any CE updates will be a part of your UserTransaction and will be committed or rolled back based on J2EE transaction calls your application makes."

代码块如下所示：

```
import com.filenet.api.collection.ObjectStoreSet;
import com.filenet.api.constants.PropertyNames;
import com.filenet.api.core.Connection;
import com.filenet.api.core.Domain;
```

任何命令行输入或输出均如下所示：

```
$ cd /home/db2inst1/sqllib/java
$ java -cp db2jcc.jar com.ibm.db2.jcc.DB2Jcc -version
```

新术语和重要词语以粗体显示。你在屏幕上看到的词语，例如菜单或对话框中的词语，在文本中如下所示："From the toolbar, navigate to Window | Open Perspective | Java"。

![](img/2295191c450a0c86b2d82df6cec04526_24_0.png)

![](img/2295191c450a0c86b2d82df6cec04526_24_1.png)

## 读者反馈

我们始终欢迎读者的反馈。请告诉我们你对本书的看法——你喜欢什么或可能不喜欢什么。读者反馈对我们开发你真正受益的书名非常重要。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及书名即可。





如果有一本你需要的书，并且希望我们出版，请在 www.packtpub.com 上的 SUGGEST A TITLE 表单中给我们留言，或发送电子邮件至 suggest@packtpub.com。

如果你在某个主题上具有专业知识，并且有兴趣撰写或为一本书做出贡献，请参阅 www.packtpub.com/authors 上的作者指南。

## 客户支持

现在你已成为 Packt 书籍的自豪拥有者，我们提供多项帮助，让你充分利用你的购买。

**下载本书的示例代码**

你可以从 http://www.PacktPub.com 上的账户下载你购买的所有 Packt 书籍的示例代码文件。如果你是在其他地方购买本书的，可以访问 http://www.PacktPub.com/support 并注册，以便将文件直接发送到你的电子邮件。

### 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果你发现我们的书籍中存在错误——可能是文本或代码中的错误——我们将非常感激你向我们报告。通过这样做，你可以拯救其他读者免于沮丧，并帮助我们改进本书的后续版本。如果你发现任何勘误，请访问 http://www.packtpub.com/support，选择你的书籍，点击勘误提交表单链接，并输入你的勘误详情。一旦你的勘误得到验证，你的提交将被接受，勘误将被上传到我们的网站，或添加到该标题现有勘误列表中，在该标题的勘误部分下。任何现有勘误都可以通过选择你的标题从 http://www.packtpub.com/support 查看。

### 盗版

互联网上版权材料的盗版是一个持续存在于所有媒体中的问题。在 Packt，我们非常重视对版权和许可的保护。如果你发现我们在互联网上以任何形式存在非法复制，我们将立即感谢你提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上涉嫌盗版材料的链接。我们感谢你在保护我们的作者以及我们为你提供有价值内容的能力方面的帮助。

### 问题

如果你在书籍的任何方面遇到问题，可以联系我们 questions@packtpub.com，我们会尽力解决。
