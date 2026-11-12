

精简表可以包含您、订户以及 Salesforce 认为合适的常用标准字段和自定义字段，全部位于同一个 Oracle 表中，从而避免了连接（其内部代价是复制记录数据）。如果 SOQL、报表或列表视图查询使用了这些字段或其子集，平台会自动使用精简表而不是连接来提高性能。作为额外好处，内部所需的记录大小更小，这意味着用于在平台中移动记录块的数据带宽可以得到更好的利用。

这显然是平台的一项高级功能，但值得考虑，作为在订户组织中提升查询性能的一种额外手段。

> 如果在 Force.com 环境中更改相应对象的字段类型或添加新字段，精简表不会保持同步。此外，精简表不会在沙箱环境中复制。因此，每当发生此类更改时，您或您的订户都需要再次联系 Salesforce 支持。还请注意，精简表无法打包；它们仅适用于订户组织的配置。

## 处理大型结果集

在大多数 Apex 场景中，无论是单独读取还是作为形成聚合查询结果的一部分，最多只能读取 50,000 条记录。但是，可以应用一个上下文参数，允许读取不限数量的记录。然而，此上下文不允许任何数据库更新。在本节中，我们将回顾以下三种管理大型数据集的方式：

- 在 Apex 中处理最多 50k 条结果集
- 在 Apex 中处理不限结果集
- 通过 Salesforce API 处理不限结果集

### 在 Apex 中处理最多 50k 条结果集

读取 50,000 条记录的问题之一是，您可能会触及其他管控器，例如堆限制。虽然堆限制在异步上下文中有所增加，但仍可能根据正在执行的处理而被触及。请查看以下示例，用于为每条 Race Data 记录生成附件：

```
List<Attachment> attachments = new List<Attachment>();
for(RaceData__c raceData : [select Id from RaceData__c])
attachments.add(
new Attachment(
Name = 'Some Attachment',
ParentId = raceData.Id,
Body = Blob.valueOf('Some Text'.repeat(1000)));
insert attachments;
```

> 要试用本章中的示例，您需要 Partner Portal Developer Edition 组织，它附带 250 MB 的记录和文档存储空间。

如果您尝试从执行匿名窗口执行上述代码，将收到以下错误，因为代码会迅速达到 6 MB 堆大小限制：

```
System.LimitException: Apex heap size too large: 6022466
```

此问题的解决方案是利用所谓的 SOQL FOR 循环（如下所示）；它允许记录数据以每次 200 条记录的分块形式返回到 Apex 代码，从而避免所有附件记录在堆上堆积：

```
for(List<RaceData__c> raceDataChunk :
[select Id from RaceData__c]) {
List<Attachment> attachments = new List<Attachment>();
for(RaceData__c raceData : raceDataChunk)
attachments.add(
new Attachment(
Name = 'Some Attachment',
ParentId = raceData.Id,
Body = Blob.valueOf('Some Text'.repeat(1000)));
insert attachments;
}
```

此代码完成后，将在一个执行上下文中生成超过 80 MB 的附件数据！请注意，在这样做时，上述代码在技术上确实违反了批量化规则，因为外层分块循环中包含一个 DML 语句。对于拆分为 200 条记录分块的 10,000 条记录，这将导致 50 个分块，因此将消耗 150 个可用 DML 语句中的 50 个。在这种情况下，目的可以证明手段的合理性，因此这只是需要注意的一点。

### 在 Apex 中处理不限结果集

在本节中，将创建一个新的 Race Analysis 页面，以允许用户查看动态应用于所选 Race Data 的各种计算和模拟的结果。最终，请想象一下，页面允许用户输入调整和其他参数以应用于计算，并重放赛事数据，以查看比赛可能会以何种不同的方式结束。

出于本章的目的，我们专注于仅查询 **赛事数据** 对象。标准行限制将 50,000 条记录（大约 5 场比赛的数据量）从 SOQL 查询中返回。对 FormulaForce 应用程序进行了以下更改，以交付此页面；如果您希望部署它们，这些更改存在于与本章关联的代码示例中：

- 已向 **RaceService** 类添加新的 **analyzeData** 方法。
- 已创建 **RaceDataSelector** 类来封装查询逻辑。
- 已为 Visualforce 页面创建 **RaceAnalysisController**。
- 已创建 **raceanalysis** Visualforce 页面。
- 已向 **Race__c** 对象添加新的 **Race Analysis** 列表自定义按钮。此按钮已添加到 **Season** 详细信息页面上与 **Races** 相关的列表中。

用户现在可以从相关列表中选择一条或多条 **Race** 记录进行处理，操作如下：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_406_0.png)

接下来的显示将在开始计算之前确认所选的比赛。此按钮会调用 RaceService.analyzeData 方法，并尝试查询适用的 RaceData__c 记录。

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_407_0.png)

### 生成更多 Race Data

为了演示如何读取超过 50,000 条记录，我们需要生成更多 Race Data。为此，我们可以重新运行本章早些时候运行过的一部分脚本，以生成最多 100,000 条记录（这将几乎填满 Partner Portal Developer Edition 组织）。

从匿名 Apex 提示符中，为每个比赛执行以下 Apex 代码；这应该会在 Race Data 对象中生成 100,000 条记录（您可以通过设置下的数据存储页面进行验证）：

```
TestData.createVolumeData(2017, 'Monza', 52, 4);
```

再运行上述代码八次，每次将比赛名称更改为 Austin、Sochi、Singapore、Shanghai、Spa、Suzuka、Silverstone 和 Budapest（请注意，西班牙的 Race Data 已在早些时候生成）。

请注意，若出于任何原因需要清除数据，请重复调用以下内容，然后进入回收站并删除所有组织数据：

```
TestData.purgeVolumeData(false);
```

### 利用 Visualforce 和 Apex 只读模式

开始分析按钮会调用服务来查询赛事数据并分析数据；出于本章的目的，它按以下方式输出查询的行总数：

```apex
ApexPages.addMessage(
    new ApexPages.Message(
    ApexPages.Severity.Info,
    'Just read ' + Limits.getQueryRows() +
    ' rows from RaceData__c'));
```

如上一张屏幕截图所示，选择所有 10 个比赛（单击“显示 5 个更多”链接以显示更多相关列表记录）。单击“开始分析”按钮以显示一条消息，确认处理的记录数超过了标准 50,000。

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_408_0.png)

之所以能够实现这一点，是因为 Visualforce 页面通过 apex:page 元素上的 readOnly 属性启用了只读模式，如下所示：

```xml
<apex:page
    standardController="Race__c"
    extensions="RaceAnalysisController"
    recordSetVar="Races"
    readOnly="true">
```

此模式也可以通过在 Apex 计划类或 Apex 远程操作方法上的 @ReadOnly Apex 属性来启用。顾名思义，缺点是不能有任何 DML 语句，因此我们无法更新数据库。对于此用例，此限制不是问题。请注意，通过使用 JavaScript Remoting，您可以创建带有和不带有 @ReadOnly 的方法组合。目前，此属性不能与 @AuraEnabled 同时使用。

