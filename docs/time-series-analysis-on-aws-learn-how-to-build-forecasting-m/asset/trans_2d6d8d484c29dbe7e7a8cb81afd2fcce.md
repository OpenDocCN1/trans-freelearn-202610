

Figure 15.1 – Datasource details – Continuous mode selection

5. 然后，您需要按以下屏幕截图所示配置实时数据格式：

Figure 15.2 – Datasource details – Continuous data configuration

使用以下信息填写这些字段：

- **Path to an object in your continuous dataset**: 如果您按照 *Chapter 14, Creating and Activating a Detector* 中的数据集上传部分操作，您实时数据的第一个文件将具有以下格式：`s3://<<YOUR-BUCKET>>/lookout-for-metrics/live/20210101/0000/20210101_000000.csv`，其中 `<<YOUR-BUCKET>>` 将被替换为您在上一章中创建的存储桶的名称。Amazon Lookout for Metrics 将尝试检测您实时数据集中每个对象的 S3 路径的共同模式。

- **Continuous data S3 path pattern**: 选择以下格式：`s3://<<YOUR-BUCKET>>/lookout-for-metrics/live/{{yyyyMMdd}}/{{HHmm}}/`.

- **Datasource interval**: 在下拉菜单中选择 `1 hour intervals`。此值必须与创建检测器时使用的间隔相匹配。

6. 我们希望此检测器能够利用历史数据的存在来更快地学习。将 Amazon Lookout for Metrics 指向您具有历史数据的输入数据集。如果您按照 *Chapter 14, Creating and Activating a Detector* 中的数据集上传部分操作，您历史数据的 S3 路径将具有以下格式：`s3://<<YOUR-BUCKET>>/lookout-for-metrics/backtest/input.csv`。使用此路径填写 **Historical data** 字段，并将所有其他参数保留为其默认值，因为它们是有效的。

7. 最后一步是指定 **Permissions**：在 **Service role** 部分选择 `Enter the ARN of a role`，然后粘贴您之前创建的角色的 ARN。您的角色 ARN 应具有以下格式：`arn:aws:iam::<<YOUR-ACCOUNT-ID>>:role/LookoutMetricsAccessRole`，其中 `<<YOUR-ACCOUNT-ID>>` 应替换为您 AWS 账户的 ID。

8. 滚动到屏幕底部并单击 **Next**。Amazon Lookout for Metrics 尝试访问您的数据并验证您的数据集。验证完成后，您会看到一个成功对话框，您可以在其中单击 **OK** 继续。

9. 在下一页，您将映射数据集的不同字段到 **Measures** 和 **Dimensions**。连续检测器的配置方式与回测模式相同：使用 `views` 和 `revenue` 作为度量（聚合方式为 **SUM**），`platform` 和 `marketplace` 作为维度，并使用 `timestamp` 作为数据集的时间戳。

10. 在屏幕底部，单击 **Next** 按钮以查看您的数据源配置。如果您对数据集的配置感到满意，请单击 **Save and activate** 开始在此数据集中检测异常。会显示一个对话框，提醒您一旦开始运行检测器，您将开始产生费用。在对话框中输入 `confirm` 并单击 **Activate**。

您的实时检测器现已准备就绪。我建议您也为其附加一个警报。您可以随意复制您在上一章末尾所执行的操作，将其应用到新的检测器上。

过一段时间后，您的检测器将被激活，并开始在您为连续检测配置的数据源的 S3 位置中搜索新文件。

## Preparing data to feed a continuous detector

当您在上一章中以回测模式配置检测器时，或当您在上一节中使用历史数据设置来配置连续检测器时，您将 Amazon Lookout for Metrics 指向了一个单一的 S3 位置，您可以在其中放置所有历史数据文件。电子商务数据集附带一个单一的 `input.csv` 文件，其中包含从 01/01/2021 到 18/01/2022 的所有数据（截至撰写时），但如果您愿意，您可以自由地将数据集结构化为多个文件。例如，您的历史数据可能按月分块：`input-2020-01.csv`（包含 2020 年 1 月的所有指标的文件），`input-2020-02.csv`，依此类推。作为提醒，这里是 `input.csv` 文件的一个摘录，该文件被检测器用于从历史数据中学习：

| platform | marketplace | timestamp | views | revenue |
|---|---|---|---|---|
| pc_web | us | 01/01/2021 00:00 | 298 | 89,4 |
| pc_web | uk | 01/01/2021 00:00 | 476 | 142,8 |
| pc_web | de | 01/01/2021 00:00 | 152 | 45,6 |
| pc_web | fr | 01/01/2021 00:00 | 405 | 121,5 |
| pc_web | es | 01/01/2021 00:00 | 113 | 33,9 |
| pc_web | it | 01/01/2021 00:00 | 153 | 45,9 |
| pc_web | jp | 01/01/2021 00:00 | 161 | 48,3 |
| mobile_web | us | 01/01/2021 00:00 | 438 | 131,4 |
| mobile_web | uk | 01/01/2021 00:00 | 536 | 160,8 |
| mobile_web | de | 01/01/2021 00:00 | 231 | 69,3 |
| mobile_web | fr | 01/01/2021 00:00 | 220 | 66 |
| mobile_web | es | 01/01/2021 00:00 | 283 | 84,9 |
| mobile_web | it | 01/01/2021 00:00 | 691 | 207,3 |
| mobile_web | jp | 01/01/2021 00:00 | 222 | 66,6 |
| mobile_app | us | 01/01/2021 00:00 | 498 | 149,4 |
| mobile_app | uk | 01/01/2021 00:00 | 218 | 65,4 |
| mobile_app | de | 01/01/2021 00:00 | 387 | 116,1 |
| mobile_app | fr | 01/01/2021 00:00 | 687 | 206,1 |
| mobile_app | es | 01/01/2021 00:00 | 197 | 59,1 |
| mobile_app | it | 01/01/2021 00:00 | 169 | 50,7 |
| mobile_app | jp | 01/01/2021 00:00 | 566 | 169,8 |

Figure 15.3 – Historical data excerpt

另一方面，*continuous detector* 在每次唤醒时都会搜索新文件以处理给定的时间间隔。您的检测器配置为按小时唤醒，因此它期望在每个小时的顶部都有一个新文件存在（考虑到您可以配置的用于处理时间的偏移）。

当您在 Amazon S3 中上传数据时使用的文件夹结构如下：

```
s3://<<YOUR-BUCKET>>/lookout-for-metrics/
live/20210101/0000/20210101_000000.csv
live/20210101/0100/20210101_010000.csv
live/20210101/0200/20210101_020000.csv
…
live/20220118/2200/20220118_220000.csv
live/20220118/2300/20220118_230000.csv
```

在此数据集中，每个 1 小时间隔的数据都存储在单个 CSV 文件中，其路径根据间隔命名。位于 `live/20210101/0000/` 路径下的文件（可以有多个）将包含 2021 年 1 月 1 日午夜开始的 1 小时间隔内生成的数据。

当您定义连续数据 S3 路径模式时，您可以让 Amazon Lookout for Metrics 通过指向给定对象来猜测应遵循哪些模式，也可以手动使用路径模板进行配置。此类模板可以包含使用双大括号配置的不同键。例如，先前描述的连续数据的路径模板如下：

```
s3://<<YOUR-BUCKET>>/lookout-for-metrics/live/{{yyyyMMdd}}/{{HHmm}}
```

这些双大括号中的字母代表 S3 路径中可能根据日期和时间而变化的部分（yyyy 代表当前四位数的年份，MM 代表一年中的双位数月份，等等）。请遵循此链接以了解可在路径模板中使用的占位符键：

```
https://docs.aws.amazon.com/lookoutmetrics/latest/dev/detectors-dataset.html#detectors-dataset-pathkeys
```

位于给定位置下的每个单独文件的结构与历史数据格式相同。它可以是 JSON Lines 或 CSV。以下是其中一个文件的完整内容：

| platform | marketplace | timestamp | views | revenue |
|---|---|---|---|---|
| pc_web | us | 17/11/2021 08:00 | 364 | 109,2 |
| pc_web | uk | 17/11/2021 08:00 | 360 | 108 |
| pc_web | de | 17/11/2021 08:00 | 600 | 180 |
| pc_web | fr | 17/11/2021 08:00 | 383 | 114,9 |
| pc_web | es | 17/11/2021 08:00 | 123 | 36,9 |
| pc_web | it | 17/11/2021 08:00 | 203 | 60,9 |
| pc_web | jp | 17/11/2021 08:00 | 313 | 93,9 |
| mobile_web | us | 17/11/2021 08:00 | 291 | 87,3 |
| mobile_web | uk | 17/11/2021 08:00 | 549 | 164,7 |
| mobile_web | de | 17/11/2021 08:00 | 340 | 102 |
| mobile_web | fr | 17/11/2021 08:00 | 812 | 243,6 |
| mobile_web | es | 17/11/2021 08:00 | 344 | 103,2 |
| mobile_web | it | 17/11/2021 08:00 | 228 | 68,4 |
| mobile_web | jp | 17/11/2021 08:00 | 151 | 45,3 |
| mobile_app | us | 17/11/2021 08:00 | 204 | 61,2 |
| mobile_app | uk | 17/11/2021 08:00 | 163 | 48,9 |
| mobile_app | de | 17/11/2021 08:00 | 589 | 176,7 |
| mobile_app | fr | 17/11/2021 08:00 | 196 | 58,8 |
| mobile_app | es | 17/11/2021 08:00 | 224 | 67,2 |
| mobile_app | it | 17/11/2021 08:00 | 113 | 33,9 |
| mobile_app | jp | 17/11/2021 08:00 | 122 | 36,6 |

Figure 15.4 – File content example for live data

此文件位于 s3://<<YOUR-BUCKET>>/lookout-for-metrics/live/20211117/0800/20211117_080000.csv 下，包含 2021 年 11 月 17 日上午 8 点至 9 点之间捕获的所有度量。让我们现在来看看 Amazon Lookout for Metrics 在检测器发现异常时如何公开结果。

## Reviewing anomalies from a trained detector

在本节中，我们将回顾 Amazon Lookout for Metrics 提供的不同仪表板，以帮助您了解检测器的状态以及它们检测到的异常。

