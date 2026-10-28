## 字段提取与查找

太好了！你已经到达第 2 部分《数据管理》的最后一章。到目前为止，在第 2 部分中，我们学习了如何将数据导入 Splunk、添加输入以及解析阶段的设置，并且我们了解了数据在写入磁盘之前遍历的各个阶段。我们尚未看到的是搜索阶段，而搜索阶段对于我们迄今为止在系统管理员（设置 Splunk）和紧随其后的数据管理员部分（整理数据并将其存储在索引器中）所做的工作至关重要。

毕竟，如果一切设置正确且数据已正确索引，那么将要搜索数据的用户才是真正的业务成果。例如，如果我们将销售、API 日志和系统日志编入 Splunk 索引，相应的用户就可以使用数据生成月度销售报告并通过电子邮件发送，并在 API 不可用时发出警报和/或安全团队在网络中的系统中发现了某个流程的漏洞。用例对于各种利益相关者来说是无穷无尽的。

所有这些工作都是在搜索头（SH）上完成的，SH 是 Splunk 的一个组件，它向索引器发出搜索请求并通过公开用户界面（UI）（Splunk Web）汇总结果，用于报告、警报、仪表板和其他有用的功能。在本章中，我们将学习 Splunk 中被称为字段的基本构建块，以及如何在索引时和搜索时从原始数据中提取它们，以及如何使用逗号分隔值（CSV）和键值存储（KV Store）查找通过额外信息丰富数据。如果你是（或将成为）数据管理员，你将几乎每天都使用字段。

在本章中，你将学习以下主题：

- 了解字段和查找
- 创建搜索时字段提取
- 创建索引时字段提取
- 创建查找

让我们从了解字段和查找开始。

### 了解字段和查找

在本节中，我们将详细了解字段和查找。让我们先从字段开始。

#### 字段

Splunk 中的**字段**讲述了数据的故事，可用于搜索和得出所需的结果，例如报告、警报和仪表板。Splunk 中的原始数据按其源类型定义编入索引为单个事件。字段是数据管理员在搜索时和索引时过程中从原始数据中提取出来赋予数据特定部分的名称。Splunk 默认情况下会为数据源分配预定义字段，例如 `host`、`source`、`sourcetype`、`splunkserver`、`_time` 等。

例如，取通话记录日志，其中包含特定时间点交换的电话号码。让我们将日志数据中的字段命名为 `time_of_call`、`caller`、`callee` 和 `duration`。

你可以通过构建每日通话次数和最长通话时长报告，并在未激活国际服务的情况下拨打国际号码时发出警报。只需编写搜索查询并使用相应的 Splunk 功能，例如报告、警报、仪表板等。

让我们来看一下示例通话记录日志：

```
2023-04-08T09:30:15, +615551234567, +615552345678, 120
2023-04-08T10:15:42, +615551234567, +615553456789, 60
2023-04-08T11:20:30, +615552345678, +15551234567, 180
2023-04-08T12:45:10, +615553456789, +615552345678, 240
```

对于第一个事件，如果我们配置了搜索时字段提取，则搜索结果中会出现以下字段/值：

- `time_of_call = 2023-04-08T09:30:15`
- `caller = +615551234567`
- `callee = +615552345678`
- `duration = 120`

让我们回顾一下关于字段的一些必知事实：

- 字段名称区分大小写。例如，`time_of_call` 和 `TIME_OF_CALL` 是不同的。
- 用户选择的搜索模式会影响字段提取的行为以及搜索结果中返回的字段。
  - **快速模式：** 在此模式下，字段发现被禁用，仅返回默认字段和你搜索的特定字段。
  - **详细模式：** 此模式发现并返回所有可用字段。
  - **智能模式：** 此默认模式发现并返回所有字段。对于转换命令（例如 `stats`、`chart` 等），它表现得像快速模式，否则表现得像详细模式。
- 搜索时使用的字段值不区分大小写 - 例如，`index=web_logs http_method="get"` 和 `index=web_logs http_method="GET"` 搜索产生相同的结果。
- 字段可以具有不同数据类型的值，例如字母数字（a）和数字（#）。请参阅 *Figure 11.1* 了解符号。
- 字段可以在索引过程中提取并存储。这意味着它们会永久存储在磁盘上的索引文件中，这会占用额外的存储空间。
- `_raw`（原始数据事件）、`_time`（事件发生时间）和 `_indextime`（事件被索引的时间）Splunk 内部字段名称由 Splunk 用于内部处理，用户在搜索数据时可以访问它们。
- 预定义/默认的 `host`、`index`、`linecount`、`punct`、`source`、`sourcetype`、`splunk_server` 和 `timestamp` 字段提供有关数据的基本信息。这些字段默认分配给每个事件。
- Splunk 默认情况下会在搜索时即时自动提取 **键值（KV）** 对和 JSON 数据格式。`props.conf` 中的 `KV_MODE` 和 `AUTO_KV_JSON` 设置使其能够工作。
- Splunk 允许你在搜索时和索引时过程中创建自定义字段提取。搜索时字段提取过程提供了一个 UI，用于为非结构化数据和通过分隔符的结构化数据创建正则表达式。我们将在即将到来的部分中讨论这两种类型的提取。

让我们看看字段在 Splunk 搜索结果中的外观，如下图所示：

![](img/5319cbd6f078c6cda7fac6347f77a07d_204_0.png)

Figure 11.1: Fields in Splunk search results

上图显示了字段在 Splunk 中的外观，其中显示了默认字段、自定义提取字段（method = GET）、自动提取字段（namespace = search）以及数据类型符号（a 表示字母数字，# 表示数字）。

#### 查找

查找是从外部源加载到 Splunk 中以丰富数据额外信息的。最常用的查找是 CSV 文件格式的内容和 KV Store（由 MongoDB 处理）。读取查找文件的流行 Splunk 搜索处理语言（SPL）命令是 input lookup 和 lookup。Splunk 支持以下类型的查找。我们将在本章中学习有关 CSV 和 KV Store 支持的查找的内容：

- 地理空间查找
- 外部查找
- CSV 查找
- KV Store 查找

地理空间查找包含与世界各地相关地点的经度和纬度等地理空间信息。默认情况下，Splunk 随附美国信息和世界各国信息。你可以在搜索和报告应用程序中发出搜索请求 - | input lookup geo_countries - 并单击可视化选项卡，然后选择 Choropleth Map。

**外部查找**，也称为**脚本化查找**，通过连接到外部源（例如 API）为 Splunk 中的事件获取额外信息。

**CSV 查找**是最流行的查找，通常为 .csv 格式。数据管理员/用户定义包含在 Splunk 中搜索数据时所需额外信息的 CSV 文件以进行丰富。CSV 查找本质上是静态的；因此，最佳做法是保持它们简单且尺寸较小。我们将在最后一节学习 CSV 查找的创建。

例如，通话记录日志包含以数字为前缀的国家代码，例如 +61、+1 等；但是，它没有国家名称。可以按如下方式定义并加载到 Splunk 中的 CSV 查找名称 – country_code_to_name_mapping.csv：

```
country_code, country_name
+61, Australia
+1, USA
```

报告可以通过将此查找与国家代码匹配来使用它，并为相关利益相关者检索国家名称以增强可读性。

**KV Store 查找**由 MongoDB 处理，MongoDB 是一种 NoSQL 文档型数据库。KV Store 查找由称为 **集合** 的配置支持。信息以 KV 对的形式存储在集合中，并且集合中的每一行都由字段 `__key` 唯一标识。KV Store 查找适用于大型查找并且需要频繁更新。

例如，与 IP 地址、DNS 和域相关的威胁情报源需要对名为 threat_intel 的查找进行持续更新。threat_intel 查找由安全团队在排查组织网络日志中存储在 Splunk 中的潜在漏洞时使用。

在本节中，我们了解了 Splunk 字段对于搜索和检索数据的重要性。我们介绍了默认和内部字段，并使用通话记录日志的示例演示了自定义字段。我们还讨论了大小写敏感性、KV 对的自动提取以及 Splunk Web 的正则表达式和分隔符提取方法。此外，我们还探讨了不同类型的查找文件类型，即将到来的部分将重点介绍 CSV 和 KV Store 查找。

在下一节中，我们将学习搜索时字段提取。

### 创建搜索时字段提取

SH 在执行搜索时提取中发挥着至关重要的作用。当用户发出搜索请求时，SH 执行搜索并作为流程的一部分执行搜索时字段提取。

SH 在搜索执行期间将定义的提取规则或模式应用于原始数据。它根据这些规则动态地从事件中提取相关字段及其关联值。然后提取的字段用于进一步分析、过滤、可视化或其他操作。





# 图 11.2：Splunk Web 字段提取器启动

图 11.2 显示了我为演示而索引的 callrecords 源类型日志。点击 Extract Fields 会打开 Select Method 页面，如图 11.3 所示：

# 图 11.3：字段提取器 – 选择方法

# 保存

为提取命名并设置权限。

图 11.5：字段提取器 – 保存 REPORT

3. 图 11.5 显示了保存的最终详细信息；在高亮的框中，我为 REPORT 命名为 callrecords-extract 并将 Permissions 设置为 Owner，这意味着提取仅对用户私有。剩余的 Source type、Fields 和 Delimiter 区域显示各自的详细信息以便在点击 Finish 按钮前进行核实。最后，点击 Finish 按钮，这将保存提取并返回 Success! 屏幕。

4. 让我们验证我们创建的字段是否已正确提取。执行 index=main sourcetype=callrecords Splunk 搜索，你将被呈现事件。展开（>）其中一个并观察被提取的字段和值，如图 11.6 所示：

# 图 11.16：查找定义 – 添加新

6. 图 11.16 显示 Destination app 为 search，为定义输入的名称为 phone_no_country_code_to_name，Type 默认填充为 File-based，并且从 Lookup file 下拉菜单中，我选择了我们在步骤 3 中上传的查找表文件。最后，点击 Save 将带你到显示 Successfully saved 消息的页面：

![](img/5319cbd6f078c6cda7fac6347f77a07d_221_0.png)

# 图 11.17：查找定义保存成功

7. 图 11.17 显示在 search 应用中保存的定义（phone_no_country_code_to_name），并且 Sharing 设置为 Private，Permissions 选项可与其它用户共享。

就这样，我们已成功在 Splunk 中创建了一个查找。要验证我们迄今为止完成的工作，请在 search 应用下打开一个搜索窗口并执行 | inputlookup phone_no_country_code_to_name 查询。如图 11.18 所示，你将被呈现 country_code 和 country_name 的表格格式：

Figure 11.18: Lookup verification

在下一节中，我们将学习关于 KV Store 查找的内容。

# KV Store 查找

KV Store 查找需要在创建查找定义之前预先创建集合。Splunk Web 不提供通过 UI 创建集合的功能。相反，一个 Splunk 支持的应用 – Splunk App for Lookup file editing（又名 Lookup Editor）– 在 splunkbase.com 有助于创建它。

按照 https://splunkbase.splunk.com/app/1724 上的应用安装步骤并在你的 Splunk 实例中安装它。该应用为 KV Store 查找的创建执行以下高层步骤：

- 创建一个集合（`collections.conf`）
- 创建一个查找定义

我们使用与上一节中创建的相同的 `country_code`、`country_name` 查找也用于 KV Store：

- 1. 安装后，从 Apps 菜单启动 Splunk App for Lookup File Editing。在右上角，点击 Create a New Lookup | KV Store Lookup。图 11.19 显示了选项。它将打开一个创建新查找的页面：

```
inputlookup phone_no_country_code_to_name
```

| country_code | country_name |
|---|---|
| +61 | Australia |
| +91 | India |
| +1 | USA |
| +47 | Norway |
| +358 | Finland |
| +33 | France |
| +44 | UK |

# 图 11.21：查找集合 – 已创建

![](img/5319cbd6f078c6cda7fac6347f77a07d_224_0.png)

4. 要从搜索访问集合，需要创建 transforms 段。为此，点击图 11.22 左上角的 Lookups 菜单项：

![](img/5319cbd6f078c6cda7fac6347f77a07d_224_1.png)

# 图 11.22：查找 – 创建转换

5. 如图 11.22 所示，搜索我们在之前步骤中创建的 kv_country_code_to_name 查找并点击 Open in Search 链接。这将打开一个 Create Lookup Transform 弹出窗口。在 Transform name 文本框中输入 kv_country_code_to_name 并点击 Create Transform 按钮。

6. 现在，在 Search App 下打开一个搜索栏并执行 | inputlookup append=t kv_country_code_to_name 查询。

如图所示 Figure 11.23，你将被呈现我们在步骤 3 中创建的结果：

![](img/5319cbd6f078c6cda7fac6347f77a07d_224_2.png)

# 图 11.23：KV Store 查找 – 最终验证

```
| inputlookup append=t kv_country_code_to_name
```

并创建了 transforms 以在搜索中访问查找。我们了解到该应用可以比标准的 Splunk Web 方法更轻松地创建 CSV 和 KV Store 查找。

我想借此机会向你表达我衷心的感谢，感谢你陪我走过本书的篇章。探索 Splunk 的复杂性和其各种功能是一件愉快的事，我希望你能从这些章节中获得价值和见解。

当你继续你的 Splunk 之旅时，我祝你在追求 Splunk Certified Admin 考试的道路上一切顺利。记住要保持专注，彻底复习材料，并练习动手练习以加强你的技能。

记住学习是一个持续的过程，总有更多东西可供发现。我鼓励你继续探索并扩展你对 Splunk 的理解，利用其功能来解锁新的见解和解决方案。再次感谢你陪我走过这段旅程。愿你在 Splunk 领域的努力充满成功和有意义的发现。

在下一章中，你将获得一份模拟考试，用于在实际考试格式的 56 道题中进行自我评估。我们还将在最后讨论答案以检查你的尝试并刷新你的理解。在下一节中，你已获得示例问题及其答案，以测试你对 Splunk 字段和查找的理解。

# 自我评估

与其它章节一样，本节是关于自我测试你迄今为止学到的主题知识。你有 10 道题，答案在问题之后给出。如每次重申的那样，如果你难以回答问题，请随时再次复习相应章节。让我们开始：

- 1. 字段名称是否区分大小写？
  A. 是
  B. 否

- 2. Splunk 在搜索时是否自动从键值格式中提取字段/值？
  A. 是
  B. 否

- 3. 选择 Splunk 中可用的查找文件类型（全部选择）：
  A. JSON 查找
  B. CSV 查找
  C. KV Store 查找
  D. 地理空间查找

4. 关于 CSV 查找，哪些陈述是正确的？全部选择：
A. CSV 查找将信息存储在 MongoDB 中
B. CSV 查找文件具有 .csv 扩展名
C. CSV 查找文件用于通过与 Splunk 中现有事件关联来丰富信息。
D. CSV 查找包含带标题的逗号分隔值信息

5. Splunk Web 中用于搜索时字段提取创建的两种方法是什么？
A. 正则表达式
B. KV 对提取
C. 分隔符提取
D. JSON 默认提取

6. Splunk Web 中的正则表达式方法是否自行生成正则表达式？
A. 是
B. 否

7. 索引时字段在 Splunk 索引中的存储是否永久？
A. 是
B. 否

8. 识别 Splunk 在索引阶段提取并存储的默认字段（全部选择）：
A. source
B. sourcetype
C. host
D. splunk_server

9. 我们能否为结构化内容如 CSV 和 PSV 类型在 UF 上设置 INDEXED_EXTRACTIONS？
A. 否
B. 是

10. 创建自定义索引时字段提取是否占用索引器上的额外存储空间？
A. 否
B. 是




