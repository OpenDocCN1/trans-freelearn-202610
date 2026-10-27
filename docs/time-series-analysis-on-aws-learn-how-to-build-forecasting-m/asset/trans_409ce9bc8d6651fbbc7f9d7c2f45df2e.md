

![](img/e4ee48104c730845e796d8c469dfeecc_335_0.png)

图 12.4 – 模板已部署

部署完成后，以下资源将已部署到您的账户中。

![](img/e4ee48104c730845e796d8c469dfeecc_335_1.png)

图 12.5 – Lookout for Equipment 仪表板部署概览

模板部署完成后，您可以立即开始使用。

## 使用 Lookout for Equipment 仪表板

模板部署完成后，使用屏幕左上角的 **Services** 下拉菜单打开 AWS 服务列表。在 **Management & Governance** 部分查找 **CloudWatch** 服务，并单击其名称进入其控制台。在那里，单击左侧菜单栏上的 **Dashboards** 链接。这将带您进入一个页面，其中列出了您在账户中可以访问的所有 CloudWatch 仪表板，如下面的屏幕截图所示。您应该会找到两个新仪表板，用于可视化您的 Amazon Lookout for Equipment 模型和调度器。

![](img/e4ee48104c730845e796d8c469dfeecc_336_0.png)

*图 12.6 – 新增的 CloudWatch 仪表板*

您可以使用名称以 **Lookout-for-Equipment-Models** 开头的仪表板来管理账户中在 Amazon Lookout for Equipment 中训练的模型。另一个仪表板可让您对账户中配置的任何调度器执行相同操作。

## 使用模型仪表板

单击名称以 **Lookout-for-Equipment-Models** 开头的仪表板。这将打开一个汇总了您账户中所有模型的仪表板。

> **重要说明**
> 
> 此仪表板使用 **CloudWatch Custom Widget** 功能，该功能在后台调用 Lambda 函数以显示自定义图表。首次打开此类仪表板时，您会看到一条警告消息，询问您在执行这些函数之前是否信任它们。单击仪表板右上角的 **Execute them all** 按钮运行这些函数并绘制组件内容。

默认情况下，仅显示过去 3 个月内训练的最后 50 个模型，如下所示：

| 数据集 | 模型 | 训练状态 | 模型仪表板 |
|---|---|---|---|
| chiller | chiller-model | SUCCESS | Create | View |
| deep-dive-demo-expander | deep-dive-demo-expander-model | SUCCESS | Create | View |
| demonstration | demonstration-model | SUCCESS | Create | View |
| equipment-asset1 | model-asset1-63b165be-b431-4ce8-94fd-8bed71b21e33 | FAILED | No model found |
| equipment-asset2 | model-asset2-2f959ce3-c6e8-49f8-9d91-c36f7a24b4d1 | FAILED | No model found |
| my_dataset | my_model | SUCCESS | Create | View |
| - | my_model2 | SUCCESS | Create | View |
| - | my_model3 | FAILED | No model found |

图 12.7 – Lookout for Equipment 模型仪表板

> **重要说明**
> 请确保您在创建模型的同一 AWS 区域中运行此 CloudWatch 仪表板。

您可以使用屏幕右上角的日期选择器来限制显示的模型。此日期选择器将使用模型训练日期来筛选要显示的模型。在此组件中，您的模型信息将按以下方式显示：

- 第一列将列出您账户中当前 AWS 区域的所有*数据集*。
- *模型名称*列在第二列中。如果多个模型使用同一数据集构建，则数据集名称将在它们之间共享，且不会重复。
- 在第三列中，*训练状态*可让您了解此模型是否成功训练。只有成功训练才能使用第四列中的操作。

- 最后一列为您提供两种可对成功训练的模型执行的操作：
  - 您可以请求为已训练模型*创建特定仪表板*。这将为此模型创建一个新的 CloudWatch 仪表板，并且还会配置并启动一个 **CloudWatch Synthetics** canary，该 canary 会将仪表板截图发送到您部署 CloudFormation 模板时配置的电子邮件地址。专用模型仪表板和 Synthetics 创建可能需要几秒钟。然后，模型列表仪表板将刷新，**View** 按钮将启用。
  - 当给定模型存在仪表板时，**View** 按钮将启用（此时 **Create** 按钮将禁用）。单击此按钮可导航到该模型专属的 CloudWatch 仪表板。

当您单击任何已训练模型的 **View** 按钮时，您将进入另一个专门用于该模型的 CloudWatch 仪表板。每个此类仪表板都会预配置多个组件：

- 模型详细信息
- 检测到的异常
- 聚合信号重要性
- 信号重要性演变

要了解有关通过这些组件获得的洞察的更多信息，请参阅*后处理检测到的事件结果以构建更深入的洞察*部分。

## 使用调度器仪表板

在 CloudWatch 仪表板主页上，单击名称以 **Lookout-for-Equipment-Schedulers** 开头的仪表板：这将打开一个汇总了您账户中所有调度器的仪表板。与模型仪表板一样，默认情况下仅显示当前 AWS 区域中过去 3 个月内创建的最后 50 个调度器，如下面的屏幕截图所示：

![](img/e4ee48104c730845e796d8c469dfeecc_339_0.png)

图 12.8 – Lookout for Equipment 调度器仪表板

在此仪表板中，您可以 **Start** 或 **Stop** 调度器。

> **重要说明**
> 请记住，运行调度器会产生费用：不要忘记停止您不再使用的调度器。

与已训练模型的评估类似，您也可以请求创建专门用于调度器的特定仪表板。创建调度器仪表板时，您还会创建一个 CloudWatch Synthetics canary，该 canary 会每周（或每天）将仪表板截图发送到您部署 CloudFormation 模板时配置的电子邮件地址。此 canary 默认配置为每周一上午 6:00 UTC 运行（如果您选择每日选项，则在每个工作日上午 6:00 UTC 运行）。

CloudWatch 专用仪表板和 Synthetics canary 可能需要几秒钟来创建，然后调度器列表仪表板将刷新，**View** 按钮将启用。

当您单击现有推理调度器的 **View** 按钮时，您将进入另一个专门用于该调度器的 CloudWatch 仪表板。已预配置两个组件：

- 调度器详细信息
- 上次执行诊断

在下一部分中，您将了解可以从这些组件获得的洞察。

## 后处理检测到的事件结果以构建更深入的洞察

在本节中，您将了解通过后处理 Amazon Lookout for Equipment 提供的原始结果可以获得的洞察类型。

### 模型详细信息

此组件仅提醒您数据集名称、训练周期和评估周期，如下面的屏幕截图所示：

pump-anomaly-detection-model | 模型详细信息

| 数据集 | 训练开始 | 训练结束 | 评估开始 | 评估结束 |
|---|---|---|---|---|
| industrial-pump | 2018-04-01 00:00:00 | 2018-10-31 00:00:00 | 2018-11-01 00:00:00 | 2020-05-04 00:00:00 |

图 12.9 – 模型详细信息示例

### 检测到的异常

检测到的异常会在所选模型的评估期间显示。组件顶部的下拉菜单可让您选择要在“检测到的异常”色带上方绘制的信号，以帮助您将特定时间序列行为与 Amazon Lookout for Equipment 检测到的实际事件关联起来。

![](img/e4ee48104c730845e796d8c469dfeecc_340_0.png)

图 12.10 – 检测到的异常示例

此组件还绘制以下内容：

- 检测到的每日事件数：在某些情况下，您会看到事件数量在达到最大值之前逐渐增加。这可能是未来潜在异常的一个良好指标。
- 检测到事件的平均持续时间。

### 聚合信号重要性

此水平条形图汇总了*评估期间*最多 15 个信号的平均重要性。

虽然 Amazon Lookout for Equipment 的原生控制台允许您单击每个检测到的事件以绘制相关的信号重要性，但此图可让您了解在检测到任何事件时，是否存在对相同传感器的系统性偏向。

在下面的屏幕截图示例中，您可以看到 **sensor_51** 对检测到的事件的平均贡献远高于其他信号。绘制此信号将帮助您理解为什么该特定传感器有如此大的贡献。这也可能表明该传感器行为噪声较大，应将其移除，以训练出更稳健的模型。

![](img/e4ee48104c730845e796d8c469dfeecc_341_0.png)

图 12.11 – 聚合信号重要性示例

