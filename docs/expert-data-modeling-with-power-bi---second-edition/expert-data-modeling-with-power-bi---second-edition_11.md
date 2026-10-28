## 索引

### A

- absolute position 630
- Access 128
- Advanced Editor 102
- AdventureWorksDW2019 database 586
- Adventure Works organization 531
- Agg Awareness 414
- aggregation, managing 435–437
- aggregation table, creating 429, 430
- aggregation table, setting 433–435
- approaches 427
- dimensions storage mode, setting 433–435
  - relationships, creating 432
  - tables, loading in DirectQuery mode 430, 431
  - using 426–428
- aggregation, at Date level
  - control measures, creating in base table 418
  - Internet Sales table, summarizing 415, 416
  - new measures, creating in summary table 418
  - relationships, creating 416, 417
  - summary table, hiding 419–421
- aggregations
  - implementing, at Date level 415
  - implementing, at Year and Month level 421–426
  - implementing, for non-DirectQuery data sources 414
  - important notes 449, 450
  - managing 435–437
  - testing 438
  - using 413, 414
- aggregation, testing options
  - DAX Studio 442, 443
  - Performance Analyzer 438–442
  - SQL Server Profiler 443–446
- alternate keys 544
- Area Chart 56
- artificial intelligence (AI) algorithms 196
- Assume referential integrity 609, 610
- Azure Active Directory (Azure AD) 496
- Azure Analysis Services (AAS) 23, 82, 167–170, 578
- Azure Data Factory (ADF) 82
- Azure Data Lake Gen 2 (ADLS Gen2) 603
  - reference link 554
- Azure Data Lake Storage Gen2 (ADLS Gen 2) 553
- Azure Data Studio 618
- Azure Machine Learning (Azure ML) 554
- Azure SQL Database 25

### B

- base table
  - control measures, creating 418
- bidirectional relationships 360–363, 376
  - avoiding 376–379
- Binary data 331
- Boyce-Codd normal form 27
- bridge table
  - hiding 376
  - using, in many-to-many relationship 369–375
- Bronze data sources 175
- Business Intelligence (BI) 177, 602
- business keys 544

### C

- calculated columns
  - avoiding 391–394
- calculated tables 323–328
  - creating 30–35
  - creating, in Power BI Desktop 35
- calculation groups
  - DAX functions 484
  - format string issue, fixing 482–484
  - implementing, to handle time intelligence 475–481
  - requirements 474
  - terminology 475
  - testing 482
  - using 474
- carriage return (CR) 137
- chaining 579
- chain length 580–586
- color coding 386
- Column by Delimiter
  - splitting 191–194
- column distribution 107–109
- column profile 110
- column quality 104, 105
- columns
  - adding, from examples 199–201
  - duplicating 201–203
  - merging 194, 195
- comma-separated values (CSV) files 128, 134–139
- Common Data Model (CDM) 553
  - Dataverse 554
  - reference link 554
- Common Data Services (CDS) 82, 173
- Composite Key 312
  - handling 350–354
- composite models 577
  - working, with DirectQuery for Power BI datasets 586–592
- composite primary keys 255
- configuration tables
  - dynamic color coding with measures 386–391
  - segmentation 384, 385
  - using 384
- connection modes
  - Connect Live mode 177
  - Data Import mode 176
  - DirectQuery mode 177
  - working with 175
- connection, types
  - Connect Live 167
  - DirectQuery 167
  - Import 167
  - report-level measures 167
- Connect Live mode 177
  - applications 178
  - limitations 178
- Currency column 256
- custom columns
  - adding 196–199
- custom connectors software development kit (SDK) 81
- Customer Relationship Management (CRM) 170
- Customer table 256–259
- custom functions 118–123
  - recursive function 124, 125
- custom types 90

### D

- data
  - loading, into Datamarts 605–608
  - obtaining, from common data sources 127, 128
- Data Analysis Expressions (DAX) 29, 82, 488
  - time dimension, creating with 74–76
  - used, for generating date dimension 66–69
  - versus Power Query 267
- Dataflows 25, 553, 554
  - computed entities, creating 565–567
  - computed tables 555
    - creating 555, 556
  - entities, creating 557–562
  - exporting 569
  - in Power BI 7, 8
  - fields 555
  - importing 570
  - incremental data refresh, configuring 567, 568
  - linked tables 555
    - linked tables, creating from 563–565
  - no-code/low-code experience 570–573
  - query plans 573–575
  - regular tables 555
  - tables 555
  - terminologies 555
  - using, scenarios 554
- Data Import mode 176
  - applications 176
  - limitations 176
- Datamart Editor 604
- Datamarts, analyzing 610
- Datamarts 25, 162–164, 601, 602
  - analyzing, in Datamart Editor 610
  - analyzing, in SQL client tools 618–620
  - data, loading into 605–608
  - data model, building 608
  - relationships, creating 609
  - relationships, managing 609
  - RLS, implementing 620–624
- Datamarts, in Datamart Editor
  - no-code experience, with Visual query 610–617
  - SQL query experience 618
- Datamarts, relationships
  - Assume referential integrity 609, 610
- data modeling
  - in Power BI 9
  - in Power BI Desktop 317, 318
  - RLS and OLS 488
  - time intelligence 48
- data model layer, Power BI 3
- Data view 4
- Model view 6
- data models
  - building, in Datamarts 608
  - building, in Power BI 10, 11
  - folder, using 402
  - insignificant objects, hiding 395
  - measure tables, creating 398–402
  - organizing 394
  - size, reducing by auto date/time disable 406–409
- data, obtaining from data sources
  - AAS 167
  - CSV files 134–139
  - Dataverse 173, 174
  - Excel 140
  - folder 128–133
  - OData Feed 170–173
  - Power BI dataflow 160–162
  - Power BI Datamarts 162–164
  - Power BI datasets 152–160
  - SQL Server 164, 165
  - SSAS 167
  - TSV files 134–139
  - TXT files 134–139
- data preparation, best practices
  - case sensitivity, in Power Query 283
  - data type conversion 296
  - key columns, defining in queries 312
  - naming conventions, using 313, 314
  - proportion of data, loading 280–282
  - queries, organizing in Power Query Editor 294–296
  - query folding 283
  - query parameters, using 309
  - query size, optimizing 307
- data preparation layer, Power BI 3
- Data Quality Bar 104
- dataset permissions for contributors
  - setting 592
- Composite (Mixed) 578
- dataset storage modes 179, 180
  - Composite (Mixed) mode 179, 578
  - Connect Live 578
  - DirectQuery storage mode 179, 577
  - Import storage mode 179, 577
- data source certification 174
  - Bronze data sources 175
  - Silver data sources 175
- data type conversion 183–191
- data type conversion best practices 296
  - affecting, data modeling 296–303
  - columns, avoiding 304
  - including 305, 306
  - one data type, considering 306
- data type facets 296
- Dataverse 82, 173, 174
  - reference link 554
- Data view 4
- Data View pane 99–101
- data visualization layer, Power BI
  - Model view 6
  - Report view 6, 7
- Data warehouse in SQL Server 11
- Date
  - dealing with 225–230
- date dimension 262–265
  - creating 267
  - generating, with DAX 66–69
  - valid dates, detecting 48–56
- Date level
  - aggregation, implementing at 415
- Date table
  - marking, as data table 69–74
- DateTime
  - dealing with 225–230
- DateTimeZone
  - dealing with 225–230
- DAX Debug Output tool 627
- DAXEvaluationLog event 627
- DAX functions 67, 624
  - EVALUATEANDLOG() function 626–630
  - NETWORKDAYS() 625, 626
  - Window functions 630
- DAX Studio 442, 443
  - download link 302
  - using 36
- degenerate dimension 252, 256
  - dealing with 549–551
  - reference link 550
- denormalization 14–22
- Development (Dev) 113
- dimensional modeling 11
- dimension tables 237, 242
  - creating 246
- dimension tables, business requirement perspective
  - Currency column 256
  - Customer table 256–259
  - Date dimensions 262–265
  - Geography dimension 247–250
  - Product dimension 252–255
  - Sales Demographic 260–262
  - sales order 250–252
  - Time dimension 265–267
- DirectQuery 160
  - composite models, working for Power BI datasets 586–592
- DirectQuery for live connections





允许，将 Power BI 数据集在 Power BI 服务中 578

启用 578

用于 PBI 数据集和 AS 的 DirectQuery

启用，在 Power BI Desktop 中 579

DirectQuery 模式 23, 177

应用程序 177

局限性 177

表，加载 430, 431

动态度量选择

实现，使用 Fields Parameters 63-66

动态 RLS 实现 503

团队成员数据，经理在父子层次结构中访问 506-511

未经授权的用户，限制访问数据 503-506

用户的登录数据，从另一个源获取 512-519

## E

增强的行级安全性编辑器 491

Enter data 功能 384

企业资源规划 (ERP) 128, 170, 280

EVALUATEANDLOG() 函数 626-630

Excel 10, 128, 140

Excel 文件

存储，在本地驱动器 140-146

存储，在 SharePoint Online 147

SharePoint Online 上的 Excel 文件，方法

Excel 文件路径，直接从 SharePoint 获取 151, 152

Excel 文件路径，从 Excel 桌面应用获取 147-150

显式度量 346

表达式 83

External Tools 442

提取部分 218

提取、转换和加载 (ETL) 25, 218, 533-535, 553

提取、转换和加载 (ETL) 工具 128

Azure Data Factory 291

SQL Server Integration Services (SSIS) 291

## F

方面 301

无事实事实表 370

阶乘计算 124

事实表 12, 237, 242

创建 268-276

精选表 322, 323

字段参数功能 595-601

字段 329

列 334

自定义格式 331-333

数据类型 329-331

层次结构 342, 343

度量值 343

字段，列

分箱 334-337

计算列 334

分组 334-337

属性 337-342

字段，度量值

显式度量 346

隐式度量 344-346

文本度量 346-348

Fields Parameters 56

实现动态度量选择 63-66

过滤行 204-207

过滤传播 358-360

固定维度 534

文件夹

创建，在多个表中 403

度量值，放置在多个表中 404, 405

Folder 数据源 128-133

外键 350

函数值 88

## G

地理维度 247-250

全局唯一标识符 (GUID) 82

Gold/Platinum 数据源 175

Group By 功能

使用 207-210

## H

混合表 23, 450, 451

配置，在 Power BI Desktop 中 451-457

重要说明 462, 463

## I

隐式度量 344-346

导入模式 23

非活动关系

处理 379

与表的多个直接关系 381-383

通过多重过滤路径 380, 381

增量加载 450

增量刷新 450, 451

重要说明 462, 463

策略，在 Power BI Desktop 中配置 451-457

测试 458-461

INDEX() 函数 632-639

推断维度 610

信息技术 (IT) 554, 602

不显著的对象

列，隐藏 398

隐式度量，隐藏 398

键列，隐藏 397

未使用的字段和表，隐藏 395-397

Internet Sales Last Month (LM) 56

Internet Sales Last Month to Date (LMTD) 48

- Internet Sales Last Year (LY) 56
- Internet Sales Last Year Month to Date (LY MTD) 48
- Internet Sales Last Year to Date (LYTD) 48
- Internet Sales Month-over-Month (MoM) 差异 57
- Internet Sales Month to Date (MTD) 48
- Internet Sales 表
  汇总 415, 416
- Internet Sales Year-over-Year (YoY) 差异 57
- Internet Sales Year to Date (YTD) 48
- 迭代数据建模，方法 25, 26
- 在数据可视化中展示业务逻辑 27
- 测试业务逻辑 27
- 数据建模 27
- 基于业务逻辑的数据准备 26
- 开展发现研讨会 26
- 专业数据建模师的思维 27, 28

## J

JavaScript Object Notation (JSON) 128

## K

Kimball Group

## N

原生查询

底层 616

NETWORKDAYS() 函数 625, 626

新术语 579

链式 579

链长 580-586

无代码体验

使用 Visual query 610-615

非 DirectQuery 数据源

实现聚合 414

Notepad++

下载链接 137

数字方面 301

数字，从文本

提取 222-224

## O

对象级安全性 (OLS) 519

实现流程 519, 520

在数据建模中 488

OData 数据源表 282

OFFSET() 函数 639, 640

OLS 实现 520-522

在单个模型中 524-527

分配成员 524

验证角色 522-524

在 Power BI Service 中验证角色 524

使用，注意事项 527, 528

一对一关系 355

本地数据网关 560

开放数据协议 (OData) 10, 170-173, 280

ORDERBY() 函数 632

## P

父子层次结构 463-465

深度，识别 465-467

级别，创建 467-471

团队成员数据，经理访问 506-511

PARTITIONBY() 函数 631

分区 630

性能分析器 438-442

配置增量刷新策略 451-457

角色 488

Power BI Desktop 报表文件 (PBIX) 9, 308

Power BI 诊断端口 444

Power BI Documenter 395

Power BI 功能

比较 22

Power BI 层 2, 3

数据模型层 3

数据准备层/Power Query 3

数据可视化层 6

Power BI 许可，注意事项 22

计算组 23, 24

Dataflows 25

Datamart 25

混合表 23

增量数据加载 23

单个数据集的最大大小 23

共享数据集 24

Power BI Pro 工作区 555

Power BI Report Server

分配成员到角色 494, 495

Power BI 服务

允许对 Power BI 数据集的 DirectQuery 直连 578

分配成员到角色 493, 494

Power BI 工具和应用

下载链接 2

Power Pivot 9

Power Query

区分大小写 82

表达式 83

查询 83

类型 89

值 83

与 DAX 对比 267

使用 Pivot 表 230-235

Power Query Diagnostics 工具 292

Power Query 编辑器 90-92

高级编辑器 102

数据视图窗格 99-101

查询窗格 92

查询设置窗格 94

状态栏 101, 102

Power Query 编辑器，查询窗格

自定义函数 92

Power Query 功能，面向数据建模者 103

列分布 107-109

列配置文件 110

列质量 104, 105

Power Query，功能

添加示例列 199-201

复制列 201-203

合并列 194, 195

添加自定义列 196-199

数据类型转换 183-191

处理日期 225-230

处理日期时间 225-230

处理日期时间区域 225-230

过滤行 204-207

从文本提取数字 222-225

追加查询 210-214

复制和引用查询 218, 219

合并查询 215-218

按分隔符拆分列 191-194

替换值 219-222

使用 Group By 207-210

Power Query M 公式语言

在 Power BI 中 81

Power Query Online 25, 553-555

Premium 许可证 553

Premium Per User (PPU) 160, 458, 605

主键 350

原始类型 89

原始值 84

产品维度 252-255

生产 (Prod) 113

Project Web App (PWA) 170

## Q

查询

追加 210-214

复制和引用 218, 219

合并 215-218

查询，连接

类型 217

查询窗格

常量值 92

自定义函数 92

组 93

查询参数 92

表 92

查询折叠 283

最佳实践 285

将数据准备推送到源系统 291

数据源 284

DirectQuery 或 Dual 存储模式 284

禁用 View Native Query 292, 293

为原生查询启用 288-290

指示 285

使用 SQL 语句 286-288

查询参数 8, 63, 111-118

参数化连接 309

定义 111

为大型表在开发中限制行数 309-312

使用 309

查询计划

在 Dataflows 中 573-575

查询设置窗格 94

已应用步骤 97, 98

查询属性 95, 96

查询大小优化

禁用查询加载 309

汇总 (group by) 308, 309

删除不必要的列 308

删除不必要的行 308

## R

记录值 85

递归函数 124, 125

常规关系 367

关系

双向关系 360-363



基数 [355](355)

复合键，处理 [350-354](350-354)

创建 [416](416), [417](417), [432](432)

筛选器传播行为 [358-360](358-360)

外键 [350](350)

主键 [350](350)

使用 [348-350](348-350)

关系，基数

多对多关系 [355-357](355-357)

一对多关系 [355](355)

一对一关系 [335](335)

相对位置 [630](630)

Replacer 函数 [219](219)

报表视图 [6](6), [7](7)

RLS 和 OLS，术语 [488](488)

增强的行级安全性编辑器 [491](491)

角色 [488](488)

角色，验证 [491](491), [492](492)

规则 [489](489), [490](490)

RLS 实现

在单一模型中 524-527

使用，注意事项 527, 528

RLS 实现流程 495

RLS 实现流程，方法 496

动态 RLS 实现 503

静态 RLS 实现 496-503

角色扮演维度

实现 471-474

行级安全性（RLS） 602

在 Datamarts 中实现 620-624

在数据建模中 488

S

Sales Demographic 260, 261

销售订单 250, 252

SAP Business Warehouse（SAP BW） 175

SCD 0 类型 534

SCD 1 类型 534-543

SCD 类型 2 533, 544-549

分段 384

自助式数据准备 553

语义模型 [9](#)

Server Management Studio（SSMS） [618](#)

Silver 数据源 [175](#)

缓慢变化维度（SCDs）

处理 [531–533](#)

类型 [534–549](#)

Smart Date Key [227](#), [452](#)

雪花化 [14](#)

Splitter.SplitTextByCharacterTransition() 函数

参考链接 [121](#)

SQL 客户端工具

Datamarts，分析 [618–620](#)

SQL 查询 [604](#)

SQL 查询体验 [618](#)

SQL Server [164](#), [165](#)

SQL Server Analysis Services 多维（SSAS MD） [23](#)

SQL Server Analysis Services（SSAS） [2](#), [9](#), [167](#), [168](#), [577](#), [578](#)

SQL Server Analysis Services 表格模型（SSAS Tabular） [82](#)

SQL Server Integration Services（SSIS） [82](#)

SQL Server Management Studio（SSMS） [443](#), [458](#)

SQL Server Profiler 443-446

星型架构 11, 12

业务需求 238

维度和事实 237

维度和事实，定义 242

星型架构，业务需求

现有表之间的关联 239

搜索 Date 和 Time 的最低所需粒度 240-242

数据源中的表数量 238, 239

星型架构，维度和事实

确定潜在维度 243

确定潜在事实 243-246

星型架构建模

与事务建模对比 11, 12

静态 RLS 实现 496-503

需求 496-502

状态栏 101, 102

存储模式

DirectQuery 模式 178

双模式 178

导入模式 178

使用 178, 179

结构化列 17

结构化值 84

函数值 88

列表值 84

记录值 85

表值 86-88

子文件夹

创建 405, 406

主题专家（SMEs） 27, 173, 244, 280, 528

汇总表

隐藏 419-421

度量值，创建 418

代理键 544

T

表 318

计算表 323-328

特色表 322, 323

透视 230-235

属性 319-322

表值 86-88

制表符分隔值（TSV）文件 134-139

Tabular Editor 458

时态表

参考链接 547

文本文件（TXT） 128, 134-139

文本度量值 346-348

时间维度 265-267

创建 267

使用 DAX 创建 74-76

数据建模中的时间智能 48

周期与周期对比计算 56-63

检测日期维度中的有效日期 48-56

事务建模

与星型架构建模对比 11, 12

Transact SQL（T-SQL）语言 604

截断和加载 450

类型数字方面 301

类型值 89

U

统一资源定位符（URLs） 112

协调世界时（UTC） 228

用户验收测试（UAT） 113

用户定义聚合 414

用户的登录数据

从其他来源获取 512-519

V

值 83

原始值 83, 84

替换 219-222

结构化值 83, 84

变量 83

查看查询计划 616, 617

虚拟关系 37

虚拟表 29, 30

计算表，创建 30-35

在 Power BI Desktop 中创建计算表 35

使用 DAX Studio 36

关系 37-48

以可视化方式显示结果 [35](35)

可视化查询 [604](604)

无代码体验 [610-615](610-615)

## W

弱关系 [367](367)

WINDOW() 函数 [630](630), [640-644](640-644)

DAX 中的窗口函数

INDEX() 函数 [632-639](632-639)

OFFSET() 函数 [639](639), [640](640)

ORDERBY() 函数 [632](632)

PARTITIONBY() 函数 [631](631)

WINDOW() 函数 [640-644](640-644)

## X

XMLA 终结点 [9](9)

参考链接 [9](9)

xVelocity [301](301), [308](308)

xVelocity 引擎 [9](9)

## Y

年和月级别

聚合，在 [421-426](421-426) 处实现

## 下载本书的免费 PDF 副本

感谢您购买这本书！

您是否喜欢在移动中阅读，但又无法随身携带纸质书？您购买的电子书是否与您选择的设备不兼容？

别担心，现在每本 Packt 图书都附带一个免费的、无 DRM 的 PDF 版本。

随时随地，在任何设备上阅读。从您喜爱的技术书籍中直接搜索、复制并粘贴代码到您的应用程序中。

好处还不止这些，您还可以独家获得折扣、新闻通讯，并且每天在收件箱中收到精彩的免费内容。

按照以下简单步骤即可获得这些福利：

1. 扫描二维码或访问以下链接

![](img/5d2bce74fd6fceebed14fd1488bf7088_853_0.png)

[https://packt.link/free-ebook/9781803246246](https://packt.link/free-ebook/9781803246246)

1. 提交您的购买凭证

2. 就这么简单！我们会将免费的 PDF 和其他福利直接发送到您的邮箱
