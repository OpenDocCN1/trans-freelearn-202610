
```
{
    "color": "#DED800",
    "label": "{{cell(Clone_of_myKPI_1.result,0,\"Median\").asString()}}",
    "value": "{{cell(myKPI_1.result,0,\"median_Amount\").asObject()}}"
}
],
    "title": "",
    "customDomain": {
        "showDomain": false
```

有关 SAQL 的详细见解和说明，请参考以下开发者指南：

https://developer.salesforce.com/docs/atlas.en-us.bi_dev_guide_saql.meta/bi_dev_guide_saql/bi_saql_reference.htm

现在，我们来了解 TCRM 中批量操作所需的代码。

## 批量操作（使用 Visualforce 和 TCRM REST API）

在 TCRM 中构建和部署自定义批量操作需要投入相当多的工作。作为入门参考，请查看我的同事 Gayathri Shivakumar 撰写的这篇博客文章：

https://visioneer360.com.au/bulk-actions-the-secret-sauce-of-einstein-analytics/

例如，你需要创建一个 Batch Apex 来更新 Contact 记录：

```
global class BatchContactActionUpdate implements Database.Batchable<SObject> {
//Start Method
global BatchContactActionUpdate( set<Id> ContactRecId){
    this.ContactRecId = ContactRecId;
}
global Database.QueryLocator start(Database.BatchableContext BC) {
    return Database.getQueryLocator('Select Id From Contact Where Id In :ContactRecId');
}
```

## XMD – 高级格式设置选项

如今，TCRM 图形用户界面（GUI）为格式设置和自定义提供了更多控制，因此 XMD 已不像过去那样经常使用。Edit Dataset 页面允许你自定义记录操作，而 Dataset Fields 则支持自定义字段标签、维度值颜色等内容。

不过，在某些情况下，高级格式设置选项仍需要使用 XMD。你可以使用 XMD 自定义以下内容：

- 修改字段和值的显示标签。
- 隐藏字段。
- 设置度量的格式。
- 按指定数值乘以度量。
- 自定义行数度量标签。
- 修改图表中的维度值颜色。
- 为维度添加操作。
- 更改值表中的默认字段。
- 设置使用多个数据集时查询结果的格式。
- 自定义度量格式。
- 为度量添加后缀和前缀。
- 定义日历年的第一天。

以下代码展示了如何将 Amount 字段设置为 `$9,999.99` 的货币格式：

```
{
  "dataset": {},
  "dates": [],
  "derivedDimensions": [],
  "derivedMeasures": [],
  "dimensions": [
    {
      "conditionalFormatting": {},
      "customActions": [],
      "field": "Region",
      "label": "Sales Region",
```

```
"members": [],
"recordDisplayFields": [],
"salesforceActions": []
}
],
"measures": [
{
"conditionalFormatting": {},
"field": "Amount",
"format": {
"customFormat": "[\"$#,##0.00\",1]"
},
"label": "Initial Amount"
}
],
"organizations": [],
"showDetailsDefaultFields": []
}
```

有关使用 XMD 的详细见解和说明，请参考以下开发者指南：

https://developer.salesforce.com/docs/atlas.en-us.bi_dev_guide_xmd.meta/bi_dev_guide_xmd/bi_xmd_intro.htm

## 你可能不需要编写代码，如果……

随着 Salesforce 持续开发 TCRM，越来越多的高级查询和自定义操作可以在 GUI 中完成，从而减少使用代码的工作量。这符合 Salesforce “点击，而不是编码”的理念。

以下是一些过去需要代码、但现在可能不再需要代码的使用场景：

- 数据流、数据准备和配方：

除复杂使用场景外，数据流（JSON）或配方通常不需要代码。

有关仪表板 JSON 的详细参考，请参阅此开发者指南：

https://developer.salesforce.com/docs/atlas.en-us.bi_dev_guide_json.meta/bi_dev_guide_json/bi_dbjson_intro.htm.

- 动态参考线（简单）：
  现在可以在仪表板编辑器中完成。
- 简单的应用程序绑定：
  可以使用仪表板编辑器中的 **Advanced Editor** 构建简单绑定。
- 向透镜添加数据集：
  现在可以通过 GUI 中的 Add Dataset 功能向透镜添加其他数据集。显示效果如下：

# 新建透镜

![](img/06379e43783b9ddfee45e9da5192efe1_284_0.png)

图 10.3 – 向透镜添加数据集

那么，在不确定是否需要编写代码时该怎么办？

## 模棱两可的情况

你越熟悉使用 TCRM GUI 构建透镜和仪表板，就越能理解哪些情况下需要代码。最佳实践是尽可能使用 GUI，仅在无法避免时才使用代码。此外，请务必及时了解最新的 TCRM 版本，因为 GUI 的功能会随着每个版本的发布而增强。

最后再分享一个我使用代码的例子——在本例中，我自定义了 JSON 数据流，使其每次运行数据流时都能重置并刷新日期：

```
"relativeDates2": {
    "action": "computeExpression",
    "parameters": {
        "source": "substituteNewDate",
        "mergeWithSource": true,
        "computedFields": [
            {
                "defaultValue": "0",
                "precision": 18,
                "name": "dateAsEpochSec2",
                "saqlExpression": "case when 'Date_of_cancellation' is null then null else Date_of_cancellation_sec_epoch end",
                "scale": 0,
                "label": "dateAsEpochSec2",
                "type": "Numeric"
            },
            {
                "defaultValue": "0",
                "precision": 18,
                "name": "dateDifference2",
                "saqlExpression": "case when 'Date_of_cancellation' is null then null else dateAsEpochSec2 - 1522127025 end ",
                "scale": 0,
                "label": "dateDifference2",
                "type": "Numeric"
            },
            {
                "defaultValue": "0",
                "precision": 18,
                "name": "dateAsEpoch2",
                "saqlExpression": "case when 'Date_of_cancellation' is null then null else dateDifference2 + date_to_epoch(now()) end ",
                "scale": 0
```

## 使用 TCRM 进行
## 仪表板设计的
## 最佳实践

现在你已经能够在 TCRM 中构建功能强大的仪表板，但你怎么知道*应该*构建什么？仪表板设计中有哪些常见错误？有效的仪表板设计有哪些公认原则？在设计、构建和部署 TCRM 仪表板时，有哪些最佳实践？本章将详细解释这些内容，为优秀的仪表板设计提供指导和参考。

我们将介绍以下内容：

- 仪表板设计中的常见错误
- 有效仪表板构建的设计理念
- TCRM 仪表板设计的最佳实践

## 技术要求

要成功执行本章中的说明，你需要具备以下条件：

- 最新版本的 Google Chrome 浏览器（使用 TCRM 时，Chrome 是首选浏览器）
- 一个可正常使用的电子邮箱地址

请确保已登录 TCRM 开发组织。

让我们先了解仪表板设计中的常见错误。

## 仪表板设计中的常见错误

以下是我在仪表板设计方面经常看到的最常见错误：

- 过分重视形式而忽视功能——让仪表板看起来*漂亮*，却不实用、不可操作，也无法提供帮助。
- 缺少比较——没有加入目标、平均值、基准和对比信息，无法为指标提供业务背景。
- 过度使用颜色——只是为了增加颜色而使用各种丰富的色彩，却没有明确的使用目的。
- 过犹不及——在页面中添加过多图表和指标，导致用户迷失和困惑。
- 缺乏故事性——向查看者提供了大量指标、图表、比较和洞察，却没有清晰的叙事主线。
- 杂乱——缺少留白会妨碍查看者理解和解读所展示的可视化内容。
- 忽视预期业务成果——在没有明确业务重点和交付成果的情况下，为了构建仪表板而构建仪表板。
- 使用不恰当或错误的可视化方式——谁愿意尝试解读一个包含 17 个分区的环形图？
- 令人困惑且不直观的设计——仪表板没有遵循任何直观的浏览流程。
- 数据不完整——只提供能说明一半情况的信息，会让用户感到沮丧并无法全面了解情况。
- 使用不恰当的颜色——某些颜色具有特定含义，例如红色和绿色，它们会影响你对图表的理解。

