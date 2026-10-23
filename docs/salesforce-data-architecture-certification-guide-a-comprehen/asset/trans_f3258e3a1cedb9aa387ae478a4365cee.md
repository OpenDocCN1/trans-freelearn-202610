
13. 对于 Experience Cloud 站点，哪种共享功能仅适用于 Customer Community 许可类型？请选择一个答案。

+   A. 共享规则
+   B. 共享集
+   C. 手动共享
+   D. 程序化共享

14. 哪种技术用于以 OData 2.0 或 OData 4.0 格式呈现外部数据？请选择一个答案：

+   A. Salesforce Connect
+   B. Batch Apex
+   C. Bulk API
+   D. Salesforce to Salesforce

15. Universal Containers 希望通过 Experience Cloud 站点向客户展示报价（Quote）。站点用户需要哪种许可类型才能查看标准的 Quote 对象记录？请选择一个答案：

+   A. Customer Community
+   B. Customer Community Plus
+   C. Partner Community

16. Universal Containers 有大量 Container Location 记录需要根据与每个客户最后一条 Order 记录关联的值，在 Salesforce 中每夜进行更新。鉴于该操作需要查询数量不确定的记录，应使用什么来实现这一要求？请选择一个答案：

+   A. Batch Apex
+   B. Bulk API
+   C. 平台外 ETL 作业
+   D. 串行模式的 Bulk API

17. Universal Containers 希望公开 Individual 对象，用于记录客户的数据隐私和保护偏好。要实现此目的，应将 Individual 字段添加到哪些对象的页面布局中？请选择三个答案：

+   A. Lead
+   B. Contact
+   C. Account
+   D. Person Account
+   E. Opportunity

18. Universal Containers 希望检测异常的登录活动，例如不可能行程（impossible journeys，即用户似乎在很短的时间内从相距甚远的多个位置登录 Salesforce）。他们还希望跟踪某些对象上超过 20 个字段的更改。可以使用哪些 Salesforce 技术来满足这些要求？请选择两个答案：

+   A. Salesforce Classic 加密
+   B. Salesforce Shield 事件监控
+   C. Salesforce Shield 加密
+   D. Salesforce Shield 字段历史跟踪

19. Universal Containers 正在实施 Salesforce 作为其新的主数据系统。基于跨多个后端系统的单一事实来源（SSOT），用户将看到的客户数据视图被称为什么？请选择一个答案：

+   A. 黄金记录
+   B. 主记录
+   C. SCV 记录
+   D. 客户主数据

20. Universal Containers 的用户在尝试对自己拥有的 Account 记录执行操作时抱怨性能不佳。Universal Containers 的每个用户通常拥有超过 10,000 条 Account 记录，但他们无法重新分配记录以解决所有权倾斜（ownership skew）问题。改善性能的推荐下一步最佳做法是什么？请选择一个答案：

+   A. 删除用户的角色，或将他们放在角色层次结构顶部的各自角色中
+   B. 使用快速操作调用 Batch Apex 作业来执行这些操作
+   C. 让系统管理员代表用户执行这些操作

## 基于视觉大模型的智能图文转换

输入一张图片，自动提取图片中的文字信息，识别文档结构和排版布局，以 Markdown 格式输出。

支持丰富的文档元素：

+   - 段落（paragraph）
+   - 标题（title）
+   - 列表（list）
+   - 表格（table）
+   - 引用（quote）
+   - 插图（image）
+   - 代码块（code）

输出 JSON 格式，包含元素类型、Markdown 内容和相对坐标（bbox）。

> > 注意：bbox 使用相对坐标，即图片宽度和高度的比例值。

```
{
  "direction": "horizontal|vertical",
  "contents": [
    {
      "type": "paragraph|title|list|...",
      "markdown": "in markdown format",
      "bbox": [0.xmin, 0.ymin, 0.xmax, 0.ymax]
    }
  ]
}
```

适用于扫描件、截图、照片等多种图像场景，可广泛用于文档电子化、信息提取与知识管理。

4. *Universal Containers* 正在向他们的 Salesforce 实现中加载大量数据。对于他们将加载的、跨多种对象类型的数百万条记录，什么被认为是最佳实践？请选择两个答案：

+   A. 尽可能对数据进行预处理，以避免不必要的计算并最大限度地减少加载后的处理时间
+   B. 不要事先对数据进行预处理，以避免不必要的计算并最大限度地减少加载后的处理时间
+   C. 延迟共享计算，以避免每条记录提交到 Salesforce 数据库时进行不必要的共享重算
+   D. 不要延迟共享计算，以避免每条记录提交到 Salesforce 数据库时进行不必要的共享重算

5. *Universal Containers* 正在使用 Bulk API 将数据加载到他们的 Salesforce 实例中。对于使用 Bulk API 加载数据，哪种模式被认为是最快的？请选择一个答案：

+   A. 串行模式
+   B. 并行模式

6. *Universal Containers* 希望能够同时向个人和企业销售。Salesforce 的哪项功能最适合利用开箱即用的对象模型，同时提供针对个人记录商机（Opportunity）的功能？请选择一个答案：

+   A. 自定义 `Individual_People__c` 对象，向 `Opportunity` 记录添加查找（lookup）字段，以将其关联到 `Individual People` 对象记录
+   B. 为所有个人设置一个总括账户（bucket account）记录，并将个人表示为该总括 `Account` 记录的联系人
+   C. 标准 Account/Contact 模型已经提供了此功能
+   D. `Person accounts`

7. 在启用 `Person` 账户之前，`Contact` 对象允许哪些 OWD（组织级默认值）值？请选择两个答案：

+   A. 由父项控制
+   B. 私有
+   C. 公开只读
+   D. 公开读写

8. *Universal Containers* 会处理一些与多个账户有关联的个人，因此他们正在考虑为此启用 **Contacts to Multiple Accounts（联系人关联多个账户）** 功能。启用 **Contacts to Multiple Accounts（联系人关联多个账户）** 功能后，联系人的活动记录的默认行为是什么？请选择一个答案：

+   A. 它们仅与创建活动记录时所查看的 Account 记录相关
+   B. 它们会被处理为与该联系人的所有 Account 记录相关
+   C. 它们汇总到该联系人的主 Account 记录
+   D. 它们都是孤立的——没有父账户

9. *Universal Containers* 希望转向基于团队的销售模式。Salesforce 的哪项功能应考虑用于允许其他用户有选择地访问 Account、Contact 和 Opportunity 记录，或作为默认销售团队的一部分访问这些记录？请选择一个答案：

+   A. 客户团队
+   B. 用户层级
+   C. AccountContactRelation 条目
+   D. AccountContactRole 条目

10. 使用客户团队授予用户访问 Account、Contact 或 Opportunity 记录的权限时，Access Levels 必须设置为何值？请选择一个答案：

+   A. AccessLevel 选项列表值必须设置为高于相应对象 OWD 的值
+   B. AccessLevel 选项列表值可以设置为相应对象 OWD 以外的任何值
+   C. AccessLevel 选项列表值必须设置为低于相应对象 OWD 的值

11. *Universal Containers* 正在将数百万条 Task 记录加载到他们新的 Salesforce 实例中。所有相关对象都已加载，因此所有相关 ID 都能够与这些 Task 关联。在加载 Task 时，出现了记录锁定问题。缓解此问题的最佳实践是什么？请选择两个答案：

+   A. 按关联/最终父级 Account ID 记录对所有 Task 记录进行排序
+   B. 按关联/最终父级 Account ID 记录对 Task 记录进行分批，并并行运行多个批次
+   C. 加载 Task 记录时无需考虑关联/最终父级 Account ID 记录，并顺序运行多个批次
+   D. 按关联的 WhatId 记录对所有 Task 记录进行排序

12. 当 Salesforce 执行索引搜索时，如果查询的筛选条件符合以下哪种情况，会使用标准索引？请选择一个答案：
A. 少于前 100 万条记录的 30%（且少于其余记录的 15%），最多 100 万条记录
B. 少于前 100 万条记录的 30%（且少于其余记录的 25%），最多 100 万条记录
C. 少于前 100 万条记录的 25%（且少于其余记录的 20%），最多 100 万条记录
D. 少于前 100 万条记录的 25%（且少于其余记录的 25%），最多 100 万条记录

13. 您已为 Account 对象上的多个字段启用了自定义索引。自定义索引在查询筛选条件符合以下哪种情况时使用？请选择一个答案：
A. 少于总记录数的 10%，最多 500,000 条记录
B. 少于总记录数的 30%，最多 333,333 条记录
C. 少于总记录数的 10%，最多 333,333 条记录
D. 少于总记录数的 10%，最多 500,000 条记录

14. 关于任务锁定行为，以下哪项陈述是正确的？请选择一个答案：
A. 当插入一条 Task 记录时，Account 记录以及 WhoId（而非 WhatId）字段引用的记录会被锁定，但仅当 Task 记录的状态不是“已完成”，并且活动日期已设置（不等于 null）时
B. 当插入一条 Task 记录时，Account 记录会被锁定，但 WhoId 和 WhatId 字段引用的记录不会被锁定，但仅当 Task 记录的状态不是“已完成”，并且活动日期已设置（不等于 null）时
C. 当插入一条 Task 记录时，Account 记录以及 WhoId 和 WhatId 字段引用的记录会被锁定，但仅当 Task 记录的状态是“已完成”，并且活动日期已设置（不等于 null）时
D. 当插入一条 Task 记录时，Account 记录以及 WhoId 和 WhatId 字段引用的记录会被锁定，但仅当 Task 记录的状态不是“已完成”，并且活动日期已设置（不等于 null）时

15. 什么是外部查找？请选择一个答案：
A. 外部查找适用于外部对象数据是您在 Salesforce 中持有的子记录的父记录的情况
B. 外部查找适用于外部对象数据是您在 Salesforce 中持有的父记录的子记录的情况
