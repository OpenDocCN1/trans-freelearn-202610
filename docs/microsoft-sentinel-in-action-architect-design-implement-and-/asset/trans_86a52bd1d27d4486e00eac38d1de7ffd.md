

## 了解更多

本节包含与页面头部书本图标相同的所有链接。如需复习，请参阅*帮助*（书本图标）部分。

此处重复这些信息是为了方便您首次进入**日志**页面时参考这些链接。一旦运行查询，结果窗口将展开并覆盖此区域，因此您需要使用书本图标才能访问这些链接。

您已经了解了 Microsoft Sentinel **日志**页面的不少内容，以及如何选择和运行内置查询来获取结果。您还学习了如何进一步操作这些结果，以及如何更改它们的显示方式。

现在是时候开始学习如何编写自己的查询了。您已经从*第5章，使用 Kusto 查询语言*掌握了 KQL 的基础知识，那么现在让我们来看看编写查询时需要考虑的步骤。

## 编写查询

既然您已经了解了如何在 Microsoft Sentinel 中使用**日志**页面，那么是时候运用您的新技能来编写自己的查询了。无论查询是什么，创建查询都需要几个基本步骤：

1. 明确您要查找的信息。您是否需要知道当前哪些计算机处于活动状态？用户在 SharePoint 中执行了哪些操作？引入了哪些数据？这将让您了解需要查看哪些日志。有关跟踪这些数据的一种方法，请参阅*第11章，Microsoft Sentinel 中的威胁搜寻*。
2. 一旦您确定了要查看哪个表，下一步就是查看该表中的少量行，以更好地了解其中存储的数据。

   最简单的方法之一是在**表**窗格中找到该表，将鼠标悬停在它上面，然后在弹出窗口中点击**查看预览数据**链接。这将显示所选日志中最多 10 行数据。查看其中一些行。这些是所需的部分信息，还是可能是全部信息？
3. 如果是，现在您就可以使用该表并应用可能需要的任何筛选来创建查询。
4. 最后，确定这些结果的最佳显示方式。如果您在分析查询中使用这些结果，则必须以表格视图返回。但是，如果这是用于威胁搜寻或在工作簿中使用，那么图形视图可能更好。

既然您已经了解了如何编写自己的查询，下面提供一些不属于 Microsoft Sentinel 但可能对您有用的查询。在阅读这些查询时，请在脑海中记住开发这些查询需要哪些步骤。需要哪个表？需要从表中获取哪些信息？如何筛选？如何显示？

## 引入的可计费数据

您已经看到了基于所引入数据量获取饼图的查询。虽然这很有用，但它只显示占总量的百分比；并不会显示所引入数据的大小。

如果您想查看过去 31 天内引入了多少可计费数据，请使用以下查询：

```
Usage
| where TimeGenerated > startofday(ago(31d))
| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by
bin(TimeGenerated, 1d), Solution
| render barchart
```

这将查看 Usage 日志，该日志保存了过去 31 天内有关日志引入的所有信息，并检查 IsBillable 列以确保引入的数据是可计费的。如果您想查看所有数据（无论是否可计费），请删除以下行：

```
| where IsBillable == true
```

然后，它按吉字节（Quantity 字段以兆字节存储信息，因此除以 1024）、数据引入的日期以及数据来源的解决方案来汇总数据。最后，它以堆叠柱状图的形式生成这些数据，以便于查看。

如果您想查看来自单个解决方案的数据量，可以在其他以 where 开头的行之后添加以下行，将 Security 替换为您想查看的任何解决方案：

```
| where Solution == "Security"
```

此查询对于确保您引入了计划引入的数据量非常有用。

## 登录的地图视图

另一个与工作簿中的地图可视化结合使用时很有用的查询（有关更多信息，请参阅第8章，创建和使用工作簿）是查看所有 Azure 登录并获取纬度和经度以便在地图上映射位置的查询。此查询将查看所有登录，并在地图上显示用户登录位置的摘要：

```
SigninLogs
| extend latitude = toint(LocationDetails.geoCoordinates.latitude), longitude = toint(LocationDetails.geoCoordinates.longitude)
| extend country = strcat(tostring(LocationDetails.countryOrRegion)," - ",tostring(LocationDetails.state))
| summarize count() by latitude, longitude, country
```

此查询将查看 SigninLogs 日志中的所有条目，该日志保存了所有登录信息。由于代码中没有应用时间限制，它将使用基于“时间范围”下拉菜单的默认时间限制。

无需传入长字符串来获取纬度和经度，它为纬度和经度创建了新变量。使用 toint() 函数将 LocationDetails.geoCoordinates.latitude 从动态数据类型转换为整数数据类型，以便它们可以在末尾的 summarize 命令中使用：

```
| extend latitude = tostring(LocationDetails.geoCoordinates.latitude)
```

经度变量将进行相同的转换。

这里展示了 KQL 的一个新特性：使用句点访问存储在动态类型中的信息。就纬度值而言，它是 geoCoordinates 值的一部分，而 geoCoordinates 值又是 LocationDetails 值的一部分，因此需要使用句点沿变量链向下遍历以获取所需的值。

## 问题

1. 查看开箱即用查询的两种方式是什么？
2. 如果我正在查看查询结果，该结果显示网络上所有计算机的列表，如何在不更改查询的情况下筛选结果以仅显示特定计算机？
3. 查看表条目预览的最简单方法是什么？
4. 在查看所有结果页面时，在一页上显示最多 200 个结果的最简单方法是什么？
5. 在查看单个结果页面时，如何在一页上显示最多 200 个结果？

## 延伸阅读

- Azure Monitor 中的日志查询概述：
  
  `https://docs.microsoft.com/en-us/azure/azure-monitor/logs/log-query-overview`

- Azure Monitor 中的日志查询入门：
  
  `https://docs.microsoft.com/en-us/azure/azure-monitor/logs/get-started-queries`

- Azure Monitor 中的日志：
  
  `https://docs.microsoft.com/en-us/azure/azure-monitor/logs/data-platform-logs`

- 优化 Azure Monitor 中的日志查询：
  
  `https://docs.microsoft.com/en-us/azure/azure-monitor/logs/query-optimization`

## 第3部分：安全威胁搜寻

在本部分中，您将学习如何使用可用工具创建分析、搜寻威胁并响应安全事件。

本部分包含以下章节：

- 第7章，创建分析规则
- 第8章，创建和使用工作簿
- 第9章，事件管理
- 第10章，配置和使用实体行为
- 第11章，Microsoft Sentinel 中的威胁搜寻

## 分析规则的类型

目前有五种类型的规则：**计划、Microsoft 安全、机器学习、Fusion 和异常。**每种类型的规则都有其特定的用途。让我们逐一探讨。

### 计划

顾名思义，这些规则按设定的计划运行以检测可疑事件。例如，您可以让规则每隔几分钟、每小时、每天或按其他间隔运行一次。这些规则的查询将使用 KQL 来定义它们要查找的内容。这些规则将在您的分析规则中占很大比例，如果您使用过其他**安全信息和事件管理 (SIEM)**系统，这些可能是您最熟悉的规则。

### Microsoft 安全

Microsoft 安全规则用于根据其他 Microsoft 安全解决方案生成的警报创建 Microsoft Sentinel 事件。

> **注意**
>
> Microsoft 最近对其部分 Azure 安全产品进行了更名。虽然大多数文档已更新为使用新名称，但 Microsoft Sentinel 规则模板尚未更新。在以下列表中，规则模板的实际名称会与产品新名称（括号中）一起注明。建议您在根据模板创建警报规则时，将其重命名为使用新名称以避免混淆。

在撰写本文时，以下安全解决方案的警报可以通过以下方式传递：

- **Microsoft Defender for Cloud Apps**
- **Microsoft Defender for Cloud**
- **Microsoft Defender for Identities**
- **Microsoft Defender for Endpoint**
- **Microsoft Defender for Office 365**
- **Microsoft Defender for IoT**

