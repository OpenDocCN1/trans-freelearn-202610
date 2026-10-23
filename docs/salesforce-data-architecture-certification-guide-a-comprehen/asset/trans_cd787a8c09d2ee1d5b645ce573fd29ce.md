
6. 在某个 Salesforce 实例中启用 Salesforce Shield 后，Field Audit Trail 数据可保留多长时间？选择一项答案：

+   A. 5 年
+   B. 7.5 年
+   C. 10 年
+   D. 12.5 年

7. 在不额外订阅产品的情况下，Salesforce 提供哪三项功能来保留对 Salesforce 实例所做更改的可追溯性？选择三项答案：

+   A. Setup Audit Trail（设置审计跟踪）
+   B. Weekly Report Exports（每周报告导出）
+   C. Field History Tracking（字段历史记录跟踪）
+   D. Event Monitoring（事件监控）
+   E. Field Audit Trail（字段审计跟踪）

8. 默认情况下，Salesforce Connect 支持哪些协议？选择两项答案：

+   A. OData 2.0
+   B. SOAP
+   C. OData 4.0
+   D. OAuth

9. *Universal Containers* 拥有两个独立的 Salesforce 实例，他们希望连接这两个实例，以便在一个实例中显示另一个实例的数据。哪种技术可以快速实现这一需求？选择一项答案：

+   A. 中间件，如 MuleSoft
+   B. Salesforce 到 Salesforce 连接器
+   C. Salesforce REST API
+   D. 自定义 Apex 逻辑

10. 以下哪些 Salesforce 产品需要连接器才能与核心 Salesforce 平台进行数据读写？选择三项答案：
A. Marketing Cloud
B. Pardot
C. B2B Commerce
D. Service Cloud
E. Experience Cloud

11. 关于任务（Task）记录锁定行为，以下哪项陈述是正确的？选择一项答案：
A. 删除任务时，Account 记录以及 WhoId 和 WhatId 字段引用的记录不会被锁定
B. 删除任务时，Account 记录以及 WhoId 和 WhatId 字段引用的记录会被锁定，无论任务记录值如何
C. 删除任务时，仅 WhoId 和 WhatId 字段引用的记录会被锁定，无论任务记录值如何
D. 删除任务时，Account 记录以及 WhoId（但不包括 WhatId）字段引用的记录会被锁定，无论任务记录值如何

12. Universal Containers 有一个面向外部用户的 Experience Cloud 站点。他们希望所有用户都能访问 Events，并让一部分用户拥有 Events 的编辑权限。要实现这一需求，需要哪些许可证类型？选择两项答案：
A. Customer Community
B. Partner Community
C. Customer Community Plus

13. Universal Containers 已实施 Enterprise Territory Management。由于区域结构的变化，部分区域需要重新分配。要将重新计算及其性能影响降至最低，应遵循的最佳实践流程是什么？选择一项答案：
A. 先从单个分支中最底层的区域开始，然后逐级向上处理层次结构
B. 先从分支中最顶层的区域开始，然后逐级向下处理层次结构
C. 先从层次结构中最底层的区域开始，然后逐级向上处理层次结构
D. 先从层次结构中最顶层的区域开始，然后逐级向下处理层次结构

14. 判断正误：在 Enterprise Territory Management 中，对子区域应用继承规则可以提高性能。选择一项答案：

+   A. 正确
+   B. 错误

15. Universal Containers 在其部分 Enterprise Territory Management 规则中设置了筛选条件。哪些是规则条件获得最佳性能的注意事项？选择两项答案：

+   A. 仅对数值字段定义条件
+   B. 不要将数值字段的条件定义为文本字符串
+   C. 仅对字符串字段定义条件
+   D. 仅将数值字段的条件定义为文本字符串

16. Universal Containers 希望将 Salesforce 实例中的数据备份到本地。哪些选项可以立即使用（无需支付额外费用）来实现这一需求？选择三项答案：

+   A. Data Export Service（数据导出服务）
+   B. Salesforce Backup and Restore（Salesforce 备份与恢复）
+   C. Report Export（报表导出）
+   D. Data Loader
+   E. List Views（列表视图）

17. Universal Containers 希望从生产 Salesforce 实例备份元数据。哪些选项可以立即使用（无需支付额外费用）来实现这一需求？选择三项答案：

+   A. Change Sets（更改集）
+   B. Sandbox Refresh（沙箱刷新），用于从生产环境创建沙箱
+   C. Force.com Migration Tool
+   D. Salesforce Metadata Export（Salesforce 元数据导出）
+   E. Salesforce Org Backup（Salesforce 组织备份）

18. Universal Containers 正在向他们的 Salesforce 实例批量加载并更新数百万条记录。在减轻此活动期间任何性能影响方面，哪些被视为最佳实践？选择两项答案：

+   A. 延迟共享计算
+   B. 禁用任何自动化，例如触发器和工作流规则
+   C. 启用自动化，例如触发器和工作流规则
+   D. 启用共享计算

19. 哪些功能需要 Salesforce 支持人员才能启用？选择三项答案：

+   A. Big Objects
+   B. Skinny Tables
+   C. Divisions
+   D. Custom Indexes
+   E. Selective Filter Conditions

20. 为什么 Skinny Tables 可以加快包含数百万行数据的对象的查询性能？选择三项答案：

+   A. 它们避免了 Salesforce 数据库中标准字段和自定义字段之间的连接
+   B. 它们不包含软删除的记录
+   C. 它们不包含对象的所有字段
+   D. 它们仅包含已索引的字段
+   E. 运行查询时，对象的其他字段会被丢弃

## 小结

在本章中，我们涵盖了大量不同的、类似考试的题目，以测试您对考试所涵盖内容的掌握程度。请在第 17 章“实践题答案”中核对您的答案。

在第 14 章“速查表”中，我们将介绍一些复习辅助工具。

接下来，让我们看看多对多关系：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_212_0.png)

图 14.2 – 多对多关系建模

接下来是共享模型：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_212_1.png)

图 14.3 – 共享模型

最后，我们再来回顾一下对象和字段存储：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_213_0.png)

图 14.4 – 对象和字段存储

现在让我们继续学习主数据管理。

## 主数据管理

现在让我们看看在“主数据管理（MDM）”主题中引入的图表。

以下内容可用作主数据管理主题的辅助工具，其中我们探讨了以 Salesforce 作为黄金记录关联持有者的 MDM 场景：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_213_1.png)

图 14.5 – Salesforce 持有黄金记录关联

![](img/7cca8452e237cf62a2d0e00a080d3fe9_215_0.png)

图 14.7 – 外部注册表持有黄金记录关联

1 从 Salesforce 发起数据更新调用
2 中间件从外部注册表查询与 Salesforce 记录关联的系统标识符
3 对与 Salesforce 记录关联的系统执行数据更新

让我们继续学习 Salesforce 数据管理。

## Salesforce 数据管理

现在让我们看看 Salesforce 数据管理复习辅助工具。

本书前面在第 4 章“Salesforce 数据管理”中已介绍过，现在让我们回顾 Experience Cloud 的许可，以便您在回答有关 Experience Cloud 和许可选项的问题时能够做好充分准备：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_216_0.png)

图 14.8 – Salesforce Experience Cloud 许可

现在让我们继续学习数据治理。

## 数据治理

在第 5 章“数据治理”中，我们介绍了企业数据治理策略的概念。

让我们回顾企业数据治理策略：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_217_0.png)

图 14.9 – 企业数据治理策略

接下来，让我们继续学习大数据量。

## 大数据量

第 6 章“理解大数据量”深入探讨了 Salesforce 中的大数据量（LDV），包括缓解技术。

