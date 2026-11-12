

一旦你怀疑某个 SOQL 查询正在引起问题，你就会对其进行性能分析，以确定正在使用哪些索引，从而决定如何解决问题、创建新索引，以及/或通过添加更多约束条件来减少过滤条件选中的记录数量。为此，你可以使用平台的一个功能，让平台在考虑为给定查询使用哪个索引（如果有的话）时解释其选择。同样，重要的是在订阅者组织或沙箱本身中使用此功能。

为本章的目的，以下示例利用 Salesforce REST API 上的 explain 参数，在我们运行前面脚本的包装组织中执行查询，以向 Race Data 对象填充 10,000 条记录。

开发者控制台包含查询规划器工具，可在首选项选项卡下启用。当你通过查询编辑器运行查询时，会弹出一个窗口，显示所考虑索引的说明。显示的信息基本上与本章中通过 explain 参数描述的信息相同。

本节在通过开发者工作台工具运行 SOQL 时利用 explain 参数。在这里，你可以在解释模式下运行各种查询并查看结果。结果显示了如果查询实际执行时会使用的一系列查询计划。它们按平台的偏好顺序显示。以下步骤展示了如何登录开发者工作台以获取给定查询的查询计划：

- 1. 登录开发者工作台。
- 2. 从实用工具菜单中，选择 REST 探索器。
- 3. 输入 /services/data/v30.0/query?explain= 然后粘贴你的 SOQL。你也可以在 URL 中使用最新的 Salesforce API 版本。
- 4. 单击执行。

首先，让我们尝试下面的示例，该示例未在 Type__c 字段上使用自定义索引。请注意，使用了 fforce 前缀；由于 SOQL 查询正在启用命名空间的包装组织中执行，你应该将此替换为你自己选择的命名空间。

```
select id from fforce__RaceData__c
where fforce__Type__c = 'PitStop Time'
```

以下截图显示了开发者工作台针对此查询显示的查询计划结果：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_398_0.png)

如预期，结果是 TableScan，与以下查询相比：

```
select id from fforce__RaceData__c
where CreatedDate = TODAY
```

以下截图显示了此查询的查询计划：

```
plans
  [Item 1]
    ▸ cardinality: 648
    ▸ fields
      ▸ 0: CreatedDate
    ▸ leadingOperationType: Index
    ▸ relativeCost: 1
    ▸ subjectCardinality: 2160
    ▸ subjectType: fforce__RaceData__c
  [Item 2]
    ▸ cardinality: 648
    ▸ fields
    ▸ leadingOperationType: TableScan
    ▸ relativeCost: 1.31666666666667
    ▸ subjectCardinality: 2160
    ▸ subjectType: fforce__RaceData__c
```

这会产生两个计划；然而，标准索引被选中，而非表扫描。

> cardinality 和 subjectCardinality 字段均基于 Salesforce 内部维护的上述统计信息进行估算，该信息涉及记录的分布情况（在此示例中，按 Type__c 区分的不同记录）。因此，它会给出过滤条件预计会选中的行数，与 SObject 中预计的总行数进行对比。
>
> 在此示例中，我们可以看到这些仅是估算值，因为我们知道 RaceData__c 对象中有 10,000 条记录；实际上，其中 1,664 条是 Sector Time 记录。不过，一般而言，只要这些值与实际物理记录大致相符，平台所做的就是计算一个值相对于另一个值的百分比，以决定索引是否值得使用。实际上，relativeCost 字段是最重要的；基本上，大于 1 的值意味着查询不会使用关联计划执行。

既然你已经大致了解如何获取这些信息，让我们再查看几个示例，但这次使用 100,000 条 RaceData__c 记录（使用相同记录分布的 10 场比赛数据）。在我的组织中，我也已要求 Salesforce 支持在 Type__c、Lap__c 和 Sector__c 自定义字段上启用自定义索引。为清晰起见，已移除了命名空间 fforce 前缀。

| SOQL | 生成的计划 |
|---|---|
| select id from RaceData__c where Type__c = 'PitStop Time' 与 PitStop Time 相关的记录数少于总记录数的 10%，因此使用了索引。 | { "plans" : [ { "cardinality" : 160, "fields" : [ "Type__c" ], "leadingOperationType" : "Index", "relativeCost" : 0.016, "sobjectCardinality" : 100000, "sobjectType" : "RaceData__c" }, { "cardinality" : 160, "fields" : [ ], "leadingOperationType" : "TableScan", "relativeCost" : 0.67013333333333334, "sobjectCardinality" : 100000, "sobjectType" : "RaceData__c" } ] } |
| select id from RaceData__c where Type__c = 'Sector Time' 由于与 Sector Time 相关的记录数大于总记录数的 10%，因此选择了表扫描计划。 | { "plans" : [ { "cardinality" : 16640, "fields" : [ ], "leadingOperationType" : "TableScan", "relativeCost" : 1.0272, "sobjectCardinality" : 100000, "sobjectType" : "RaceData__c" }, { "cardinality" : 16640, "fields" : [ "fforce__Type__c" ], "leadingOperationType" : "Index", "relativeCost" : 1.664, "sobjectCardinality" : 100000, "sobjectType" : "RaceData__c" } ] } |

select id from RaceData__c where Type__c = 'Sector Time' and RaceName__c = 'Spa'

同样，即使通过添加比赛名称使查询更具选择性，还是选择了表扫描。在这里，建议在比赛名称上添加自定义索引，因为它很可能是常见的过滤条件。

```
{
    "plans" : [ {
        "cardinality" : 1000,
        "fields" : [ ],
        "leadingOperationType" :
        "TableScan",
        "relativeCost" : 0.68833333333333334,
        "sobjectCardinality" : 100000,
        "sobjectType" : "fforce__RaceData__c"
    }, {
        "cardinality" : 16640,
        "fields" : [ "fforce__Type__c" ],
        "leadingOperationType" : "Index",
        "relativeCost" : 1.664,
        "sobjectCardinality" : 100000,
        "sobjectType" : "fforce__RaceData__c"
    } ]
}
```

select id from RaceData__c where Type__c != 'PitStop Time'

由于使用了否定运算符，因此选择了表扫描计划。

```
{
    "plans" : [ {
        "cardinality" : 100000,
        "fields" : [ ],
        "leadingOperationType" :
        "TableScan",
        "relativeCost" : 2.8333333333333335,
        "sobjectCardinality" : 100000,
        "sobjectType" : "RaceData__c"
    } ]
}
```

| SOQL | 生成的计划 |
| :--- | :--- |
| select id from RaceData__c where Type__c != 'PitStop' and Lap__c >=20 and Lap__c <40 尽管使用了否定运算符，但通过添加有自定义索引支持的更具选择性的条件，平台选择了使用索引而非表扫描。 | { "plans" : [ { "cardinality" : 10000, "fields" : [ "Lap__c" ], "leadingOperationType" : "Index", "relativeCost" : 1.0, "sobjectCardinality" : 100000, "sobjectType" : "RaceData__c" }, { "cardinality" : 40000, "fields" : [ ], "leadingOperationType" : "TableScan", "relativeCost" : 1.5333333333333334, "sobjectCardinality" : 100000, "sobjectType" : "RaceData__c" } ] } |

## 精简表

在内部，Salesforce 将给定记录的标准对象字段数据和自定义字段数据存储在两个独立的物理 Oracle 表中。因此，虽然你看不到，但当你执行 SOQL 查询以返回标准字段和自定义字段的混合结果时，这在内部需要一个 Oracle SQL 查询，并需要进行内部数据库连接。

如果 Salesforce 支持确定避免此连接可以加快给定查询集的速度，他们可以创建一个精简表。此类表对 Force.com 开发者不可见，并由平台自动与你的对象记录保持同步，包括任何标准或自定义索引。

