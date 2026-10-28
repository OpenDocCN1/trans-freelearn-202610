

# 使用图表和地图可视化数据

完成后，点击底部的 **Add** 按钮。嘿，这就完了吗？我们到底可以在哪里配置地图？在 **ACTIONS** 列中，点击 **Constructor**。是的，现在这更像一个编辑界面。首先我们需要添加一些内容，所以点击地图顶部 **Icon** 标签旁边的 **Add**。这会在地图的左上角添加一个元素。位置不是很好。解决这个问题，点击并拖动图标到其他位置，大约在 50×50 的单元格附近：

![](img/f5313a804532fabacc8d07cc8607eeba_346_0.png)

> 注意它如何吸附到网格。我们稍后会讨论这个功能。

地图看起来仍然不太有用，那现在该怎么办？只需单击一次我们刚刚添加的元素——这会打开元素属性表单。注意元素本身现在也被高亮显示了。默认情况下，添加的地图元素是图像，它不会显示有关监控系统的任何信息。为了简单起步，我们将使用主机，因此在 **Type** 下拉菜单中选择 **Host**——注意这会稍微改变表单。在 **Label** 文本区域输入 "A test host"，然后在 **Host** 字段中输入 "test" 并在下拉菜单中点击 **A test host**。默认图标是 **Server_(96)**——让我们稍微减小一点。在 **Icons** 部分的 **Default** 下拉菜单中选择 **Server_(64)**。属性应如下所示：

## 企业数据架构

本文档概述了企业数据架构策略，强调了所有数据平台的扩展性、安全性和运营效率。

- Centralized data governance framework
- Unified metadata management layer
- Real-time streaming data ingestion pipeline
- Batch processing for historical data analysis

该架构采用混合方法，结合云原生服务和本地基础设施，以确保数据驻留合规性并最大限度地降低关键工作负载的延迟。

### Key Components

- 1. **Data Lake Storage** - Serves as the central repository for raw and curated data assets.
- 2. **Processing Engine** - Supports both stream and batch processing using unified APIs.
- 3. **Data Catalog** - Provides end-to-end lineage tracking and business glossary management.
- 4. **Security Layer** - Implements row-level security and dynamic data masking.

如需更多详细信息，请参阅官方文档或联系数据平台团队。

一张只有一个元素的地图并不令人兴奋，所以再次点击 **Icon** 标签旁边的 **Add**，然后将新元素拖动到 450x50 的单元格附近。单击一次并更改其属性。首先在 **Type** 下拉菜单中选择 **Host**，然后为 **Label** 输入 Another host 并在 **Host** 字段中开始输入 "another"。在下拉菜单中选择 **Another host**。将默认图标更改为 **Server_(64)**，然后点击 **Apply**。注意元素不再与网格对齐——我们更改了图标大小，导致它们有点偏离网格单元格的中心。这是因为对齐是基于图标中心进行的，但图标定位是图标的左上角。当我们更改图标大小时，其左上角是固定的，而中心发生了变化，因此不再对齐。我们可以稍微拖动图标，图标就会吸附到网格上，或者我们可以点击顶部的 **Align icons** 控制。 现在点击 **Align icons**。同时注意地图上方的其他 **Grid controls**——点击 **Shown** 将隐藏网格（并将该标签更改为 **Hidden**）。点击 **On** 将停止在移动时将图标对齐到网格（并将该标签更改为 **Off**）。

地图不会自动保存——要保存，请点击右上角的 **Update** 按钮。出现的弹出窗口可能令人困惑——它并不是询问我们是否要保存地图。实际上，正如消息所述，地图在那时已经保存了。点击 **OK** 将返回地图列表，点击 **Cancel** 将保持地图编辑表单打开。通常点击 **OK** 或 **Cancel** 区别不大。

> 最好不时保存地图，尤其是在进行大量更改时。

现在是检查地图外观的好时机，所以前往 **Monitoring | Maps** 并在 **NAME** 列中点击 **First map**。它应该看起来相当不错，网格引导线已移除，除了大面积的空白区域，就像我们使用饼图时一样。这需要修复，所以点击地图本身上方的 **All maps** 并在 **First map** 旁边点击 **Properties**。在 **Width** 字段中输入 "600"，在 **Height** 字段中输入 "225"，然后点击 **Update**。再次在 **First map** 旁边的 **ACTIONS** 列中点击 **Constructor**。

显示和对齐到网格是可单独控制的——我们可以显示网格但不自动对齐到它，或不显示网格但仍用于对齐：

![](img/f5313a804532fabacc8d07cc8607eeba_348_0.png)

默认情况下，使用 50x50 像素的网格，并且有预定义的 20、40、50、75 和 100 的矩形网格可用。这些尺寸是硬编码的，不能自定义：

![](img/f5313a804532fabacc8d07cc8607eeba_349_0.png)

对于我们的地图，将网格大小更改为 75x75 并启用与网格的对齐，将图标定位在地图的相对两端，距离边框一个单元格。点击 Update 按钮保存更改。

> 更改地图时务必点击 Update。

前往 Monitoring | Maps 并在 NAME 列中点击 First map：

![](img/f5313a804532fabacc8d07cc8607eeba_349_1.png)

> 注意右上角的 + 按钮。点击它，地图可以轻松添加到仪表板收藏中。查看图表时也有相同的功能。

看起来好多了，我们验证了在需要向地图添加更多元素时可以轻松更改地图尺寸。

> 💡 Zabbix 地图不会像普通和堆叠图表的宽度那样自动缩放——配置的尺寸是固定的。

除了漂亮的视图外，此显示还提供什么？点击 **Another host** 图标：

![](img/f5313a804532fabacc8d07cc8607eeba_350_0.png)

在这里我们可以访问一些全局脚本，包括默认脚本和我们在 *Chapter 7, Acting upon Monitored Conditions* 中配置的几个脚本。还有快速链接到主机清单，在 *Chapter 5, Managing Hosts, Users, and Permissions* 中讨论，以及此主机的最新数据、触发器和图表页面。当我们使用这些链接时，对应的视图将被过滤以显示我们最初点击的主机的信息。此部分中的最后一个链接 **Host screens** 目前处于禁用状态。我们将在 *Chapter 10, Visualizing Data with Screens and Slideshows* 中讨论主机（或模板化）屏幕。

我们谈到了使用地图来查看事物如何连接。在进一步探索之前，让我们创建一个基本的测试基础设施——我们将创建三项和三个触发器来表示网络可用性。为了便于控制，我们将检查某些文件是否存在，然后根据需要创建和删除这些文件。在 "A test host" 和 "Another host" 上执行以下操作：

```
$ touch /tmp/severity{1,2,3}
```

在前端，导航到 **Configuration | Templates** 并在 **C_Template_Linux** 旁边点击 **Items**，然后点击 **Create item** 按钮。在 **Name** 字段中输入 "Link $1" 并在 **Key** 字段中输入 `vfs.file.exists[/tmp/severity1]`，然后点击底部的 **Add** 按钮。现在克隆此项（通过点击它，然后点击 **Clone** 按钮）并创建另外两个，根据需要将文件名末尾的数字更改为 "2" 和 "3"。

> 不要忘记在打开项目详细信息后点击 **Clone**，否则你将只是编辑现有项目。

验证你已正确设置这三项：

| NAME | TRIGGERS | KEY |
|---|---|---|
| Link /tmp/severity1 |  | vfs.file.exists[/tmp/severity1] |
| Link /tmp/severity2 |  | vfs.file.exists[/tmp/severity2] |
| Link /tmp/severity3 |  | vfs.file.exists[/tmp/severity3] |

现在是触发器——在导航栏中点击 **Triggers** 并点击 **Create trigger** 按钮。在 **Name** 字段中输入 "Latency too high on {HOST.NAME}" 并在 **Expression** 字段中输入 `{C_Template_Linux:vfs.file.exists[/tmp/severity1].last()}=0`。在 **Severity** 部分选择 **Warning**，然后点击底部的 **Add** 按钮。与项目一样，克隆此触发器两次并更改 **Expression** 字段中的严重性编号。至于名称和严重性，让我们使用以下内容：

- 第二个触发器用于 **severity2** 文件：名称为 "**Link down for 5 minutes on {HOST.NAME}**" 且严重性为 **Average**
- 第三个触发器用于 **severity3** 文件：名称为 "**Link down for 10 minutes on {HOST.NAME}**" 且严重性为 **High**

