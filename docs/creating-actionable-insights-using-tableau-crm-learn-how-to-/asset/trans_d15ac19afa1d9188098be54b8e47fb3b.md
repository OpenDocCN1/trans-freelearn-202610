

我们现在将学习如何使用 JSON 和 **用户界面 (UI)** 创建仪表板绑定。

## 使用绑定将事物联系起来

TCRM 中的绑定是什么？绑定用于在步骤和部件之间传递值——即为动态值和选择参数化值。示例用例包括以下内容：

- 动态参考线
- 动态分组
- 动态度量
- 动态排序
- 动态图表类型

需要考虑两种类型的绑定。首先，选择绑定用于根据另一查询中的选择更新查询，例如通过切换部件选择日期范围。其次，结果绑定用于根据另一查询的结果更新查询，例如使用来自 case 语句的计算值来对数据进行分桶。

TCRM 正在慢慢远离要求使用代码创建绑定的做法，但许多用例仍需要在 JSON 中进行开发工作，如下一节所述。

## JSON 中的绑定

我们现在要谈谈 JSON。你可能熟悉 JSON，但在 TCRM 的上下文中可能不熟悉。下面的图表显示了 JSON 和 TCRM 仪表板是如何配合的：

![](img/06379e43783b9ddfee45e9da5192efe1_253_0.png)

图 9.15 – JSON 和 TCRM 仪表板

在仪表板编辑器中，按 Cmd + E（或 Ctrl + E）显示仪表板背后的 JSON。JSON 只是指令的书写语言。JSON 包含所有部件/图表描述以及针对数据集运行的查询。每个图表实际上是几行 JSON，描述类型和属性以及要运行的查询。一种特殊情况是当其中一个查询直接使用 SAQL 自定义编写时。

什么是 JSON？简单来说，它是文件中用一堆用大括号 { } 括起来的键和值组成的描述性指令。值可以是文本、数值、布尔值或文本/数值的数组；你可以有多个这样的信息块。以下是 JSON 的简单分解：

![](img/06379e43783b9ddfee45e9da5192efe1_254_0.png)

图 9.16 – JSON 的简单分解

以下是来自“我的第一个仪表板”的 JSON 摘要示例：

```
"steps": {
    "Leads_by_Status_1": {
        "broadcastFacet": true,
        "datasets": [
            {
                "id": "0Fb5g000000U115CAC",
                "label": "Leads",
                "name": "Leads",
                "url": "/services/data/v52.0/wave/datasets/0Fb5g000000U115CAC"
            }
        ],
        "isGlobal": false,
        "label": "Leads by Status",
        "query": {
            "measures": [
                [
```

```
"count",
"*"
]
],
"groups": [
"Status"
]
},
"receiveFacetSource": {
"mode": "all",
"steps": []
},
"selectMode": "multi",
"type": "aggregateflex",
"useGlobal": true,
"visualizationParameters": {
"parameters": {
"autoFitMode": "keepLabels",
"showValues": true,
"bins": {
"breakpoints": {
"high": 100,
"low": 0
},
"bands": {
"high": {
"color": "#008000",
"label": ""
},
"low": {
"color": "#B22222",
"label": ""
},
"medium": {
"color": "#ffa500",
"label": ""
```

```
```json
}
}
},
"legend": {
  "descOrder": false,
  "showHeader": true,
  "show": true,
  "customSize": "auto",
  "position": "right-top",
  "inside": false
},
"axisMode": "multi",
"tooltip": {
  "showBinLabel": true,
  "measures": "",
  "showNullValues": true,
  "showPercentage": true,
  "showDimensions": true,
  "showMeasures": true,
  "customizeTooltip": false,
  "dimensions": ""
},
"visualizationType": "hbar",
"title": {
  "fontSize": 16,
  "subtitleFontSize": 11,
  "label": "Leads by Status",
  "align": "center",
  "subtitleLabel": ""
},
"binValues": false,
"trellis": {
  "flipLabels": false,
  "showGridLines": true,
  "size": [
    100,
```

```
100
],
"enable": false,
"type": "x",
"chartsPerLine": 4
},
"columnMap": {
"trellis": [],
"dimensionAxis": [
"Status"
],
"plots": [
"count"
]
},
"showActionMenu": true,
"measureAxis2": {
"sqrtScale": false,
"showTitle": true,
"showAxis": true,
"title": "",
"customDomain": {
"showDomain": false
}
},
"measureAxis1": {
"sqrtScale": false,
"showTitle": true,
"showAxis": true,
"title": "",
"customDomain": {
"showDomain": false
}
},
"theme": "wave",
"dimensionAxis": {
```

```
"showTitle": true,
"customSize": "auto",
"showAxis": true,
"title": "",
"icons": {
    "useIcons": false,
    "iconProps": {
        "fit": "cover",
        "column": "",
        "type": "round"
    }
},
"applyConditionalFormatting": true
},
"type": "chart",
"options": {}
},
"Pipeline_by_Industry_1": {
    "broadcastFacet": true,
    "datasets": [
        {
            "id": "0Fb5g000000TV2bCAG",
            "label": "Opportunities",
            "name": "opportunity1",
            "url": "/services/data/v52.0/wave/datasets/0Fb5g000000TV2bCAG"
        }
    ],
```

```
"isGlobal": false,
"label": "Pipeline by Industry",
"query": {
    "measures": [
        [
            "sum",
            "Amount"
        ]
    ],
    "groups": [
        "Account.Industry"
    ],
    "filters": [
        [
            "IsWon",
            [
                "false"
            ],
            "in"
        ]
    ],
    "order": [
        [
            "sum_Amount",
            {
                "ascending": false
            }
        ]
    ]
},
```

仔细查看此示例，你会看到它包含构建仪表板的指令。以下示例显示了基本的部件/步骤 JSON：

### 基本部件/步骤 JSON

![](img/06379e43783b9ddfee45e9da5192efe1_260_0.png)

图 9.17 – 基本部件/步骤 JSON 示例

这里，我们有一个仪表板 JSON 的简化示例：

### 仪表板 JSON

#### 示例：

![](img/06379e43783b9ddfee45e9da5192efe1_260_1.png)

图 9.18 – 仪表板 JSON 示例

让我们看一个 JSON 绑定的简单示例——动态分组。此示例来自公开可用的 *Learning Adventure* 应用，我在本文末尾提到它。请按以下步骤操作：

- 1. 创建一个新的空白仪表板。
- 2. 创建一个切换部件 (`pillbox_1`)，其步骤 (`Grouping_Toggle_1`) 指定不同的分组。
- 3. 创建一个条形图 (`chart_1`)，其步骤 (`Dynamic_Query`) 以紧凑形式按切换部件中选择的分组对总金额进行分组。
- 4. 在 Spring 2018 版本中，引入了新的 `columnMap` 图表属性来构建更强大的可视化；但是，`columnMap` 对于此动态可视化不是必需的。将此 `columnMap {}` 参数替换为 `columnMap: null`。
- 5. 为了根据切换部件中的选择动态设置图表中的分组，请打开仪表板 JSON 并绑定 Dynamic_Query 步骤中的 groups 属性——请参阅此步骤列表之后的 code snippet。
- 6. 单击 **完成**，即可开始使用。

以下是用于绑定 Dynamic_Query 步骤中 groups 属性的 JSON：

```
```json
"groups": "{{ cell(Grouping_Toggle_1.selection, 0, \"groups\").asObject() }}"
```
```

这段代码是什么意思？让我们逐步分解如下：

- `cell(Grouping_Toggle_1.selection, 0, \"groups\")` 返回来自 Grouping_Toggle_1 步骤的所选分组，并从步骤选择中获取 `cell`（第一行，“groups”列）的值。索引 0 是第一行，索引 1 是第二行，以此类推。
- `asObject()` 将结果格式化为字符串数组，这是仪表板 JSON 中预期的格式。

这就是 *Learning Adventure* 应用中示例的外观：

![](img/06379e43783b9ddfee45e9da5192efe1_262_0.png)

图 9.19 – 动态分组示例

有很多参考资料深入探讨 JSON、绑定等方面的细节。如前所述，我建议你从 TCRM 开发组织创建 *Learning Adventure* 应用——在 Analytics Studio 中选择创建模板化应用后，它看起来像这样：

![](img/06379e43783b9ddfee45e9da5192efe1_263_0.png)

图 9.20 – Learning Adventure 应用

你也可以在此处参考 *Tableau CRM Dashboard JSON Developer Guide*：

```
https://developer.salesforce.com/docs/atlas.en-us.bi_dev_guide_json.meta/bi_dev_guide_json/bi_dbjson_intro.htm
```

现在，我们将看到如何从 UI 创建一些绑定，而无需代码。

## UI 中的绑定

有几种方法可以在不进入仪表板 JSON 的情况下创建绑定。
我们将在此处看一个示例，创建动态参考线，然后看第二个示例，我们将了解 **高级编辑器**。

