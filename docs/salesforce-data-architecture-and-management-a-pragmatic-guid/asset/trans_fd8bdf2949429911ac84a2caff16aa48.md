

- 它们支持有限数量的数据类型，例如 **文本**、**数字**、**日期/时间**、**查找** 和 **文本区域**。

- 它们无法跟踪记录字段历史记录，也不能用于跟踪活动。

> **重要提示**
> 
> Salesforce 正在持续更新其大对象功能，因此请参考最新发布文档以获取最新信息。

## 外部对象

这些是用于将外部数据引入 Salesforce 的对象。假设在我们的招聘应用中，我们使用外部服务进行候选人背景调查。我们可以不将这些结果导入 Salesforce，而是简单地使用外部对象，并根据需要让我们的用户使用它们。

在下一部分，我们将探讨 Salesforce 中的一些特殊字段类型及其用例。父子关系和查找关系将在下一部分中更详细地处理，因此我们不会在此介绍它们。

## Salesforce 中的字段

Salesforce 中有多种不同类型的字段可用。在本节中，我们将介绍 Salesforce 中的特殊字段。

## 名称字段

这是记录的名称，与记录 ID 不同。这可以是自动编号或文本字段。该字段通常较短，由用户用于引用记录，而不是使用长 15 字符或 18 字符 (char) 记录 ID。大多数对象都有此字段，除了 Case 对象（没有此字段）。Case 对象只有自动编号字段。

> **自动编号字段**
> 
> 这是一个特殊字段，使用系统生成的唯一编号，每次创建新记录时该编号递增。您可以定义字段的格式及起始编号，例如，对于发票，您可以定义字段以 INV-012345 格式生成发票号。
> 
> 这也是在列表视图、搜索结果和报告中显示为可单击选项的字段。Salesforce 也允许此字段基于文本，这意味着多个记录可以具有相同的值，因为系统不再递增数字序列。

## ID 字段

这是对象的主键（关系型数据库术语），用于唯一标识 Salesforce 记录。这是一个完全唯一的字段，且没有两个记录可以具有相同的 ID。记录 ID 可以是 15 字符或 18 字符长，前者区分大小写；例如，如果 0030000000000BcA 更改为 0030000000000bCa，Salesforce 将抛出错误。在外部应用程序中使用 18 字符 ID 非常重要，因为它们可能将这两个 ID 视为相同，而 Salesforce 则将其视为不同。

## 审计字段

这些审计字段用于合规性、记录和报告对数据所做的更改，以及用于 Salesforce 的流程自动化工具，例如工作流规则、Lightning 流程构建器、流程和集成。最常用的审计字段如下：

- CreatedBy
- CreatedDate
- LastModifiedByID
- LastModifiedDate

还有一个名为 SystemModStamp 的字段，当自动化流程更新记录时会对其进行更新。此处的自动化流程指的是 Salesforce 内部代码。无论何时，当用户、自动化工具（如工作流规则、Lightning 流程或流程构建器）、API 或 DML 语句更新记录时，LastModifiedDate 和 SystemModStamp 日期都会被更新。这意味着这两个日期可能不同，且最佳实践是不要将 SystemModStamp 日期用于集成目的。除了 SystemModStamp 字段外，所有四个审计字段都可以使用 Data Loader 或类似工具进行更新。在迁移数据时使用审计字段很有帮助，以免丢失重要的审计信息，并应谨慎使用以保护数据完整性。

## 复合字段

这些是通过将多个原始数据类型分组在一起而创建的复杂数据类型。这样做是为了简化处理这些字段的代码。如果您使用过 Account 对象，Address 字段是一个复合字段，由 Street、StateCode、State、PostalCode、City、Country 和其他一些字段组成。复合字段是只读的，更改只能对底层单个字段进行。字段的复合版本仅可通过 SOAP 或 REST API 访问。

复合字段的另一个示例是 Geolocation，它是纬度和经度字段的组合。这些字段默认可用于 Account、Contact、Quote 和 User 对象，并且在填充地址时，Salesforce 会自动填充 Geolocation 字段。在以下截图中，您可以看到 Billing Address 字段的数据类型为 Address：

![](img/782d45700e093b3d82b348c0ccc8ccba_77_0.png)

### 图 2.21 – 账单地址字段

在以下截图中，我们已使用 SOQL 查询了相同的复合字段：

![](img/782d45700e093b3d82b348c0ccc8ccba_77_1.png)

### 图 2.22 – 账单地址复合字段

BillingLatitude 和 BillingLongitude 字段构成 BillingAddress 复合字段，该字段也可被查询。请注意它在 BillingAddress 下显示 [object Object]，这表明这是一个复杂数据类型。

## AnyType 字段

这些是多态字段，返回字符串、布尔值、日期、数字、URL、电子邮件或其他数据类型。这些字段用于通过 API 查询数据时，并通常用于历史记录对象。多态字段可用于获取相关对象的信息，而这些相关对象可以是不同类型。

Owner、Who 和 What 字段是 AnyType 字段的示例。以下示例显示 Who 可以是 Lead 或 Contact 类型：

| Id | Who.Id | Who.Type | Subject |
|---|---|---|---|
| 00T5A000005M2xAUAB | 0035A000003MtdeQAD | Contact | 他在回来时给 Tim 打电话 |
| 00T5A000005M2XgUAl | 00Q5A000001UKLgZUAX | Lead | 合同后续跟进 |

### 图 2.23 – 指向多个对象的 Who.Type 字段

请注意，在上述截图中，Who.Type 同时引用了 Contact 和 Lead 对象。

还可以通过在 Developer Workbench 中检查字段属性来确认：

- aggregatable: true
- aiPredictionField: false
- autoNumber: false
- byteLength: 18
- calculated: false
- caseSensitive: false
- createable: true
- custom: false
- defaultedOnCreate: false
- deprecatedAndHidden: false
- digits: 0
- filterable: true
- groupable: true
- idLookup: false
- label: Name ID
- length: 18
- name: WhoId
- nameField: false
- namePointing: true
- nillable: true
- permissionable: true
- polymorphicForeignKey: true
- precision: 0
- queryByDistance: false
- relationshipName: Who
- restrictedPicklist: false
- scale: 0
- searchPrefilterable: true
- soapType: tns:ID
- sortable: true
- type: reference
- reference to: Contact

### 2.24 – Developer Workbench 中同时引用 Contact 和 Lead 对象的 Who.Id 元数据（在图中，Reference To 字段未显示 Lead 对象）

请注意，当使用 Developer Workbench 查看 Task 对象上字段的元数据时，polymorphicForeignKey 设置为 true。

## 货币字段

这些用于存储货币数据，例如 Opportunity 对象上的 Amount 字段。当组织中启用多货币功能时，对象上的另一个字段 CurrencyIsoCode 决定字段将以何种货币显示。例如，如果 CurrencyIsoCode 为 GBP，则所有货币字段的值都将以 GBP 显示。

Salesforce 具有多货币功能，允许您使用组织默认货币以外的货币。您可以启用您开展业务的选定货币，然后组织管理员可以维护这些货币的转换率。另一个功能是高级货币管理，允许您在 Salesforce 中管理带日期的汇率。此功能存储历史汇率，从而实现准确报告。高级货币管理有一些限制，应在仔细审查其功能和业务要求后启用。

## JunctionIdList 字段

此字段类型用于查询，并可选择在 Apex 中使用返回的结果以直接修改多对多关系。当通过设置启用共享活动时，将启用此字段类型。

## 摘要

在本章中，我们了解了 Salesforce 中的不同类型的对象，并查看了它们的用途和限制。然后我们回顾了一些特定的字段类型，并研究了有关其用途的详细信息。

我们探讨了包括规范化和反规范化在内的数据库概念以及什么是数据建模。您还了解了为什么 Salesforce 与其他关系型数据库不同，至少从租户的角度来看，以及它如何针对读取操作进行了优化。接下来，我们探讨了在设计数据模型时需要考虑的一些重要设计原则。

我们研究了不同的关系及其用途、限制和用例。接下来，我们深入探讨了 Salesforce 的多租户架构、其不同组件，以及它们如何协同生成我们作为用户在运行时交互的虚拟应用程序。

