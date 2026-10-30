## 问题与答案

### 第一章

- 定义数据分析及其提供的价值。
- 列出 TCRM 的五个特性。
- TCRM 与 Tableau 有何不同？
- TCRM 的五个实际用例是什么？

数据分析是对数据进行收集、理解、共享和行动。它使组织能够从数据中获得价值并获得可操作的业务洞察。我们的目标是从数据到洞察再到**行动**。

- 与 Salesforce CRM 平台的原生双向集成
- 在数据管理器中进行平台内的数据提取、组合和转换
- 与各种其他平台和云存储提供商的外部连接
- 数据可视化、分析和探索
- 一个数据行动框架，用于基于洞察进行决策

Tableau 用于从任何地方分析和可视化数据；TCRM 提供原生的 Salesforce 体验并优化 Salesforce 数据。

- 销售业绩，例如实际值与目标的预测对比
- 按线索来源、区域和团队成员的线索转化率
- 营销活动的投资回报率
- 案例管理和服务团队绩效
- 按区域、销售区域和国家划分的全球销售分解

### 第二章

- TCRM 如何为您的组织或客户提供价值？
  这是组织特定的答案。
- 您的组织在数据分析方面表现出色吗？如果不是，什么阻碍了它做到这一点？
  这是组织特定的答案。

- 什么是 TCRM 资产？举一个例子。
  TCRM 资产是四项关键内容中的任何一项：仪表板、故事、透镜或数据集。

- TCRM 应用可以通过什么方式共享？
  三种方式之一：
  - 通过 TCRM 授予访问权限。
  - 发布到 Chatter 动态。
  - 通过应用 URL 共享。

- 什么是 TCRM 透镜？
  透镜是对数据的自包含探索；一个查询配有可视化或表格。

- 真假 – TCRM 应用是否仅限于标准、开箱即用设置。
  假。

- TCRM 主页侧边菜单上的“收藏”链接的用途是什么？
  您可以将任何 TCRM 资产，例如数据集或透镜，设为收藏并将其添加到此列表以便更轻松地访问。

- 如果开箱即用的 TCRM 应用即使在您对其进行自定义后仍不符合您的要求，该怎么办？
  TCRM 具有极强的灵活性，并支持资产的自定义开发。

- 如何在 Salesforce 环境中启用 TCRM？
  您可以通过转到设置 | 分析 | 入门并将其打开来启用 TCRM。

### 第三章

- 什么是数据同步？
  数据同步是指将数据分阶段处理以供数据流和配方使用。
- TCRM 可用的五个远程数据连接有哪些？
  - 外部 Salesforce 组织
  - NetSuite
  - Marketo
  - Google Analytics
  - Amazon Redshift
- 数据集构建器的用途是什么 – 用于从 Salesforce 数据创建数据集、从外部数据创建数据集，还是两者兼有？
  用于从 Salesforce 数据创建数据集
- 在数据集构建器中工作时，为什么要确保选择将用于添加对象关系所需的任何 ID 字段很重要？
  确保选择将用于向数据集添加对象关系所需的任何 ID 字段很重要，否则相关对象将没有键可供链接。
- 编辑数据集字段属性时给出的常见错误是什么？
  将维度错误地分类为度量

### 第四章

- 什么是数据配方，以及何时应使用它？
  TCRM 数据配方用于将多个数据集连接成一个，并通过智能建议、列配置文件和强大的转换来准备、清理和转换数据。配方非常适合几乎没有或没有数据集成经验的 Salesforce 管理员或分析师，他们希望获得有关如何清理数据的指导，并且还能预览转换结果。配方也是经验丰富的数据忍者创建数据集的强大工具，他们希望使用无代码、可视化界面。
- 在配方中何时需要追加数据？
  当您需要将一个数据集或多个数据集的行添加到另一个数据集时，就会使用追加数据。

- 配方编辑器中可用的数据集连接类型有哪些？
  查找、左连接、右连接、内连接或全外连接
- 常见计算在哪里可以找到？
  常见计算在转换工具栏中拥有自己的功能按钮 – 这些按钮会为您填入正确的列和语法公式。
- 桶转换的一个实际用例是什么？
  当您想要从两个度量创建热力图时，桶是一个有用的工具。
- 解释检测情感转换。
  它用于确定文本的情感是积极、消极还是中性，并将这些值添加到指定的列中。
- 填空：

  您必须指定包含数据层次结构中每个**字段**的字段，以及包含基于层次结构中类似的**节点**的字段，以便组合展平转换。这将为每个层次结构节点创建一个记录，该节点被称为**子**节点。

  分割转换始终创建**两个**新列。

### 第五章

- 查找连接、内连接和全连接之间的区别是什么？
  以下列表强调了这些连接之间的主要区别：
  - 右侧的查找源是用户对象，左侧的数据流是配方数据集。任何与配方数据集中的用户 ID 匹配的用户记录都将返回有关该用户的信息并将其增强到配方数据集中，而不会添加任何行。
  - 内连接用于创建两个数据源的交集或重叠，其中仅包含两个数据流都匹配的行。
  - 全连接用于在您想要为每种组合包含一条记录时使用。
- 给出使用全连接的一个实际例子。
  您想要组合产品和产品系列并满足每种可行的组合。
- 为什么需要将来自外部来源的数据与 Salesforce 对象连接起来？举一个实际例子。
  您想要将来自外部 ERP 的订单数据与 Salesforce 中的销售数据连接起来，以便您可以比较管道、已完成交易和实际值。
- 如何使用一个配方中的两个数据流组合聚合和分组？
  您可以在数据准备流程中创建多个数据流，根据需要对每个流进行聚合和汇总，然后重新连接这些流并使用连接节点增强详细数据。
- 必须选择多行公式选项才能执行窗口函数 – 什么是窗口函数？您可能会在何时使用它们？
  窗口函数用于在数据准备配方中对行进行计算，而不是对列进行计算。一个示例用例是使用基于快照日期的 lag 函数计算机会金额随时间的变化。
- 什么是展平转换，以及它的用例是什么？
  展平转换将层次结构的排序数据展平为两列。您可以在为安全谓词使用之前展平数据。

### 第六章

- 透镜使您能够做什么？
  以下列表强调了使用透镜可以完成的各种操作：
  - 探索数据集的各种特征，包括维度、度量、字段标签和名称。
  - 使用各种度量、维度、筛选器和公式对数据进行切片和切分。
  - 创建可剪切到仪表板的视觉效果和表格。
  - 创建可与其它用户共享的视觉效果和表格。
- 透镜的剪切命令的用途是什么？
  如果仪表板在仪表板编辑器中打开，则透镜将被剪切或导出到仪表板作为小部件。
- 透镜透视表的用途之一是什么？
  对数值数据进行小计和汇总，并按类别和子类别汇总数据。




## 第7章

- TCRM中应用级安全的目的是什么？

    它用于管理对应用内资产（透镜、数据集和仪表板）的访问。

- 如何通过Salesforce对象和字段访问来控制TCRM安全？

    管理员可以配置Salesforce对象和字段上的权限，以实现对象级和字段级安全，从而控制对Salesforce数据的访问。TCRM根据两个系统TCRM/Salesforce用户（集成用户和安全用户）的权限来获取对Salesforce数据的访问权限。通过Salesforce中的安全配置文件限制这些用户的访问，即可从Salesforce对象和字段访问层面控制TCRM安全。

- 当数据流作业运行时，TCRM使用**集成**用户的权限从Salesforce对象和字段中提取数据。

- Salesforce共享继承的三个限制是什么？

    以下列表强调了这些限制：

    - 仅当所有支持的对象的每条记录都少于400个共享描述符时，才能应用它。
    - 它可能会影响查询、数据流和数据准备配方的性能。
    - 为了让对象出现在安全共享源列表中，自定义对象的主键必须是数据集中的一个字段。

- 安全谓词的三个使用场景是什么？

    您可以通过以下方式控制数据可见性：

    - 角色层次结构
    - 经理层次结构
    - 用户区域

- 安全谓词表达式不能超过多少个字符？

    5,000

- 这是一个有效的谓词表达式吗？为什么，或为什么不？ "Revenue">100

    不是；数据集列和运算符之间、运算符和值之间以及逻辑运算符前后必须至少有一个空格。

- 安全谓词表达式区分大小写——正确还是错误？

    正确

## 第8章

- 如果你希望同事访问仪表板的特定视图并深入查看数据，你会如何与他们共享？

    使用共享URL。

- “将小部件复制到剪贴板”命令的目的是什么？

    它将一个或多个小部件复制到剪贴板，以便粘贴到仪表板的任何位置。

## 第9章

- 仪表板默认列数的建议值是多少？

    50

- 当您从一个仪表板页面移动到另一个页面时，应用于第一个页面的过滤器会保留吗？

    会

- 如何在仪表板中使用容器小部件？

    容器小部件可用于创建一个高亮面板，该面板可以作为一个包含一组小部件的整体进行移动和调整大小。

- 什么是自上而下的仪表板设计？TCRM页面如何促进这种设计？

    使用TCRM页面的多功能、自上而下的仪表板设计克服了几个障碍，并为对数据进行切片和切块提供了简单、灵活的界面。

- “克隆查询”命令会克隆查询并打开它以进行**编辑**。

- 给出使用TCRM页面的三个用例示例。

    页面可用于缩放图表以进行进一步分析，在主要图表上显示更多细节，或促进数据分段。

- 当您在仪表板内从一个页面移动到另一个页面时，已应用的过滤器会保留还是被清除？

    它们会保留。

- 您何时可能需要SAQL？

    当您需要进行自定义计算、即时高级数据操作、不同数据集的共同分组（连接）、顶部/底部列表，并结合绑定实现图表上的动态值时，您可能会使用SAQL。

- 关键字和函数以大写字母书写，并且它们不区分大小写。正确还是错误？

    错误

- 您在哪里编辑SAQL？

    透镜的每一步都是基于SAQL的，因此可以对它进行编辑和/或添加自定义SAQL。这些可以在JSON编辑器模式下查看，或者复制到您选择的JSON编辑器中。

- 您使用什么键盘快捷键来访问仪表板JSON？

    *Command或Ctrl + E。*

- 给出三个使用绑定使仪表板更具交互性和动态性的用例示例。

    - 动态参考线
    - 动态分组
    - 动态度量

## 第10章

填空：

- 安全**谓词**
- **JSON**中的绑定
- SAQL**表达式**
- 批量操作（使用**Visualforce**和TCRM REST API）
- **时间序列**（已修改）
- XMD – 高级**格式**选项

## 第11章

填空：

- 仔细选择您的**图表**类型。
- 切勿试图将所有可用**信息**放在同一页面上。
- 使用少量**颜色**和**色调**。
- 使仪表板尽可能易于**使用**。
- 采用明智的**布局**选择。
- 始终为您的见解提供**背景**。
- 保持**简洁**。
- 平衡功能与**创意**——有效并不意味着枯燥。
- 为用户提供所需的**细节**级别，而不是更多。
- 了解仪表板将**如何**以及在**何处**被使用。

## 第12章

- 嵌入式仪表板如何帮助用户采用TCRM？

    您将见解嵌入到CRM工作流中的用户面前，这样他们在**业务如常（BAU）**环境中工作时将不可避免地查看仪表板。

- 给出TCRM支持的三个已配置记录操作示例。

    - 创建和更新对象记录。
    - 记录通话。
    - 触发Lightning组件、流程和Visualforce页面的自定义操作。

- 通过Lightning App Builder嵌入TCRM仪表板时，仪表板过滤器的用途是什么？

    当嵌入式仪表板在布局中就绪时，它将显示所有账户的信息，而您希望它只显示正在显示的账户记录的信息。添加仪表板过滤器即可实现这一点。

- 给出一个如何从TCRM驱动Salesforce自动化的示例。

    通过创建一个以触发流程的方式配置的任务来启动Lightning流程。

![](img/06379e43783b9ddfee45e9da5192efe1_355_0.png)

Packt.com

订阅我们的在线数字图书馆，即可完整访问超过7,000本图书和视频，以及行业领先的工具，帮助您规划个人发展并推动职业发展。如需更多信息，请访问我们的网站。

## 为什么订阅？

- 通过来自4,000多名行业专业人士的实用电子书和视频，减少学习时间，增加编码时间。
- 通过专为您打造的技能计划改进学习。
- 每月免费获得一本电子书或一个视频。
- 完全可搜索，便于访问关键信息。
- 复制粘贴、打印和收藏内容。

您是否知道Packt提供每本已出版图书的电子书版本，并提供PDF和ePub文件？您可以在packt.com升级到电子书版本，作为印刷版图书客户，您有权享受电子书副本的折扣。如需更多详情，请通过customercare@packtpub.com与我们联系。

在www.packt.com，您还可以阅读一系列免费技术文章，注册多种免费新闻通讯，并获得Packt图书和电子书的独家折扣和优惠。





![](img/06379e43783b9ddfee45e9da5192efe1_357_0.png)

## 使用 Python 和 R 扩展 Power BI

Luca Zavarella

ISBN: 9781801078207

- 了解在 Power BI 产品中使用 Python 和 R 的最佳实践
- 使用 Python 和 R 在 Power BI 中执行复杂的数据操作
- 在 Power BI 中应用数据匿名化和数据假名化
- 使用 Python 和 R 在 Power BI 中记录数据并加载大型数据集
- 使用外部 API 和机器学习模型丰富你的 Power BI 仪表板
- 使用线性优化和其他算法从数据中提取洞察
- 处理多元和时间序列数据的异常值和缺失值
- 使用 R 脚本创建任意复杂的、可视化

## Packt 正在寻找像你这样的作者

如果你有兴趣成为 Packt 的作者，请访问 authors.packtpub.com 并立即申请。我们已与数千名开发人员和技术专业人士合作，就像你一样，帮助他们与全球技术社区分享他们的见解。你可以进行一般申请，申请我们正在招聘作者的特定热门主题，或提交你自己的想法。

## 分享你的想法

现在你已经完成了 *Creating Actionable Insights Using Tableau CRM*，我们很想听听你的想法！如果你从亚马逊购买了本书，请点击此处直接前往本书在亚马逊的评论页面并分享你的反馈，或在你购买的网站上留下评论。

你的评论对我们和技术社区非常重要，将帮助我们确保提供优质的内容。

C
case function 122
case statement
  used, for creating buckets 234, 236
coalesce function 123
code
  example 269, 270
  need for 254
  use cases 267, 268
  using, in TCRM 254
comma-separated values (CSV) 34, 107
Compare table 157-164
complex formula transformations
  additional functions 122
  case function 122, 123
  coalesce function 123
  custom formulas 117
  date functions 117, 118
  need for 116
  numeric functions 118
  numeric operators 118
  string functions 120
  usage 116
Window functions 121
customer relationship management (CRM) 127
customer satisfaction (CSAT) 133
custom formulas 117
custom selection bindings
  about 256
  example 256
D
Dashboard 32
dashboard design
  building 274
business, need for 276-279
common mistakes 272-274
insights, providing 274-276
limitations 282-286
principles 298-300
usage 286, 288
user engagement 293-298
visual communication 289, 290
with data 280-282
dashboard editor
  about 190-192
  options 203, 204
dashboard JSON
  reference link 267
data
  analyzing, with Table Mode 154
data analytics
  significance 4, 5
data joins
  about 101
  connected data, brining together
    by joining 107-110
  joining, Salesforce objects 104-106
  types 101, 102
data joins, types
  full outer join 103
  inner join 103
  left join 103
  lookup join 103
  right join 103
Data Prep 3.0 75
data preparation 54
Data Prep tool
  Add or Subtract Days or Months
    function, using 124
  aggregation node, usage 110, 111
  complex formula transformations 116
  data, flattening use cases 126

data joins 101
Date Difference functions, using 125
dates, working with in 123
Detect Sentiment transformation,
    using 132, 133
Predict Missing Values
    transform, using 132
usage 100
dataset 32, 55
dataset, creating from CSV data
    questions and challenges 70
dataset, creating from Salesforce
    questions and challenges 70
data story 5, 6
data sync 48, 57
data transformation, TCRM data recipe
    about 78
    attributes, editing 91
    bucket 89
    columns, dropping 91
    data, adding 78
    data, aggregating 82
    data, appending 79, 80
    data, filtering 81, 82
    date, formatting 88
    dimension to date 88
    dimension to measure 88
    discovery prediction 92
    flatten 90, 91
    formulae, applying 85, 86
    functions 86
    hands-on exercise 94-97
    hands-on exercise 94-97
    lowercase 88
    missing values, predicting 92
    outer join, creating 92, 93
    performing 78
    replace 88
    sentiment, detecting 91

split 87
substring 87
trim 87
uppercase 88
data visibility control scenarios,
    security predicates
logged-in user's country 181
manager hierarchy 181
opportunity team and
    accounts team 181
role hierarchy 181
user territory 181
date formulas 117, 118
Date widget
    adding, to sales dashboard 202
Detect Sentiment transformation 132, 133
dev org
    setting up 27-30
Direct to Consumer (DTC) 33
dynamic charts
    about 255
    example 255
dynamic reference line
    creating 248-250

## E

EA-SQL format 85
Einstein Analytics-enabled
    Developer Edition org 26
Einstein Analytics Growth 44
Einstein Analytics Plus 9, 44
embedded dashboards
    about 322, 324-327
    need for 323, 324
Enterprise Architect (EA) 117
enterprise resource planning (ERP) 107

Extended Metadata (XMD)
  about 266
  customizing 266
  reference link 267
extract, transform, load (ETL) 57

F
field availability 48
filter node 81
filters
  adding, to sales dashboard 199, 200
flat files 56
  Salesforce data, augmenting 69, 70
flatten Transformation 127-131
full outer join 103
functions
  Additional functions 87
  Date and time functions 86
  Numeric operators and functions 86
  String functions 86
  Window functions 87

G
Gestalt principles
  audience 290-292
  reference link 289

H
highlights panel
  adding, to sales dashboard 197-199

I
identifiers (IDs) 227

Image widget
  adding, to sales dashboard 201
inner join 103

J
JavaScript Object Notation (JSON)
  about 227
  bindings 236, 238, 244-247
Join node
  using 92

K
key performance indicators (KPIs) 14

L
Lens
  about 32, 140
  Chart mode, working with 141-150
  cloning 141
  creating 141
  editing 141
  using, to create TCRM analytics dashboard 167, 168
Lens usage
  tips and hints 168, 169
lifetime customer value (LCV) 20
List widget
  adding, to sales dashboard 202
lookup join 103

M
machine learning (ML) 31, 110

## N

natural language processing (NLP) 132
nested bindings
  about 257
  example 257
not-for-profit (NFP) institutions 21
numeric functions 118, 119
numeric operators 118, 119

real-world case studies, TCRM
  about 14
marketing attribution analytics 18-20
other use cases 21
out-of-the-box sales analytics 14
property development insights 17, 18
student success insights, for
  higher education 15, 16
record actions
  about 316
  configuring, in TCRM 316-321
  creating, in TCRM 316
remote data connectors
  for TCRM 60
results binding 236
right join 103

## O

org configuration 48

## P

page animations
  usage 216-227
pages
  top-down design 204-209
permission set licenses (PSLs) 44
  assigning 45
pivot table 165
prebuilt permission sets
  assigning 46, 47
Predict Missing Values transform 132
Professional Services Automation
  (PSA) 274

## Q

Query mode 166

## R

Range widget
  adding, to sales dashboard 202

## S

sales dashboard
  creating 192
  Date widget, adding to 201
  filters, adding to 199, 200
  highlights panel, adding to 197-199
  Image widget, adding to 201
  List widget, adding to 202
  Range widget, adding to 202
  widgets, placing on 193-196
Salesforce
  automation, driving from TCRM 322
  role hierarchy data type 126
Salesforce Advanced Query
  Language (SAQL)
  about 75, 166, 227
  buckets, creating with case
    statement 234, 236
  editing 228-230

  references 235
  usage 228
  Salesforce analytics tools
    using, for business use case 12
    working 11
  Salesforce data
    augmenting, with flat files 69, 70
  Salesforce data access 172
  Salesforce data, into TCRM
    about 61
    dataset builder 62-68
    dataset, designing 61, 62
  Salesforce ecosystem
    data analytics tools 11
  Salesforce objects
    joining 104-106
  Salesforce reports and dashboards
    points, for upgrading to TCRM 13
  Salesforce sharing inheritance
    about 172
    deploying 179-181
  SAQL expressions
    about 258
    averages 259, 260
    buckets (complex) 263, 264
    groupings 259, 260
    images, using as icons 260, 261
    products, grouping by 262
    reference link 264
    sample 261
    subtotals 259, 260
    summaries 259, 260
    union 258
  SAQL syntax
    usage 228
  SAQL, use cases examples
    Union use case 230-233
  search functionality 32
  security predicates
    about 172, 181, 254
    data visibility control, scenarios 181
    defining 181-184
    examples 182, 255
    reference link 255
    usage 181
  security tools, TCRM administrator
    Salesforce data access 172
    Salesforce sharing inheritance 172
    security predicates 172
    TCRM app-level security 172
  selection binding 236
  service-level agreement (SLA) 112
  Show Fields panel
    using 150, 152
  Simple Storage Service (S3) 34, 107
  string functions 120

T
Tableau CRM Dashboard JSON Overview
  reference link 247
Tableau CRM SAQL Developer Guide
  reference link 167
Tableau CRM (TCRM)
  about 3, 7
  applications 254
  architecture 7-9
  automation in Salesforce,
    driving from 322
  bindings 236
  enabling, in org 47
  examples 23
  external data, combining with
    Salesforce data 58, 59
  external data, ingesting 58, 59
  features 7





## U
- 真实世界案例研究 14
- 记录操作，配置 316-321
- 记录操作，创建 316
- 远程数据连接器 60
- 用法 10
- 用于卓越数据分析 22
- 与 Tableau 对比 12, 13
- 表格模式
  - 用于分析数据 154
- TCRM 访问权限
  - 管理 173-175
- TCRM 管理员
  - 安全工具 172
- TCRM 分析应用
  - 关于 35
  - 配置 36
  - 配置 38, 39
  - 创建 36, 37
  - 自定义 41
  - 详细设置，自定义 43, 44
  - 探索 40, 41
  - 安装 36
  - 设置，修改为自定义
    - 设置 42, 43
- TCRM 分析仪表板
  - 使用 Lens 创建 167, 168
- TCRM Analytics Studio
  - 动手练习 152-154
- TCRM 应用
  - 链接，重新配置 48
  - 故障排除 47
- TCRM 应用级安全
  - 关于 172
  - 配置 176-179
- TCRM 仪表板
  - 用户指南 210-213
- TCRM 仪表板设计
  - 最佳实践 300
  - 仪表板组件 309-311
  - 数据集 308
  - 构建 GUI 308
  - 利用页面 300-308
  - SAQL 用法 308
- TCRM 数据流 56
- TCRM 数据全景 56
- TCRM 数据配方。*参见* 数据配方，TCRM
  - 关于 74, 75
  - 制作 74
  - 创建 75-77
  - 连接数据 92, 93
  - 输出数据 93, 94
  - 转换数据 78
- 用于数据连接的 TCRM
  - 功能 54
  - 限制 60
- TCRM 安全
  - 关于 172
- Salesforce 数据权限，
  - 控制 173
  - 使用 172
- 时间序列（修改） 265
- 转换节点 84, 115
- 统一资源定位符 (URL) 28, 211
- Union
  - 示例 258, 259
  - 用例 230-233
- 用户体验 (UX) 224
- 用户界面 (UI)
  - 关于 81, 236
  - 绑定 247, 248

## V
- 值表 154-157

## W
- 小部件
  - 在销售仪表板上放置 193-196
- 窗口函数 121, 122

