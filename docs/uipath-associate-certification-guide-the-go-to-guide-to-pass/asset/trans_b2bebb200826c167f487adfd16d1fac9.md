

- 4. 程序化机器人，在 **订单录入 CRM** 网页上输入以下详细信息：
  - 客户详细信息
  - 发票详情
- 5. 对剩余的队列项目重复步骤 2、3 和 4。
- 6. 如果 Orchestrator 中没有新的队列项目，则程序化机器人成功结束流程。

让我们通过图表来了解整个流程：

![](img/84b06ad2832c53c5f677f8ce4c80f184_474_0.png)

图 15.1 – 高级别流程图

现在您已了解流程概览，让我们开始构建自动化。

## 设置项目

在开始构建流程之前，首先，让我们正确设置所有 UiPath Studio 配置：

- 1. 打开 UiPath Studio，创建名为 *Invoice_Processing_Performer* 的新流程：

![](img/84b06ad2832c53c5f677f8ce4c80f184_475_0.png)

图 15.2 – 创建新流程

- 2. 单击设计器功能区中的管理包，并将当前项目依赖项更新为以下版本：
  - UiPath.Excel.Activities - v2.10.4
  - UiPath.Mail.Activities – v1.10.5
  - UiPath.System.Activities – v21.4.1
  - UiPath.UIAutomation.Activities – v21.4.4：

![](img/84b06ad2832c53c5f677f8ce4c80f184_475_1.png)

图 15.3 – 已更新的依赖项

- 3. 现在您已将依赖项更新为正确版本，还需安装另一个依赖项，UiPath.WebAPI.Activities – v1.7.0：

![](img/84b06ad2832c53c5f677f8ce4c80f184_476_0.png)

图 15.4 – 已添加 WebAPI 依赖项

现在您已配置好所需的一切，让我们开始构建流程。

## 启动 CRM

在本节中，您将启动订单录入 CRM。在继续构建机器人之前，请从 https://github.com/PacktPublishing/UiPath-Associate-Certification-Guide/tree/main/Chapter15/Input%20File%20-%20Order%20Entry%20CRM 的 GitHub 仓库中的输入文件 – 订单录入 CRM 文件夹下载订单录入 CRM。

订单录入 CRM 具有与发票对应的字段，如下面的屏幕截图所示：

![](img/84b06ad2832c53c5f677f8ce4c80f184_477_0.png)

图 15.5 – 订单录入 CRM

在项目目录中创建名为 CRM 的文件夹，并将订单录入 CRM 页面移入此文件夹：

![](img/84b06ad2832c53c5f677f8ce4c80f184_477_1.png)

图 15.6 – 订单录入 CRM 已移入 CRM 文件夹

现在您已下载 CRM，让我们开始构建自动化：

- 1. 在 UiPath Studio 中，打开 Main.xaml 文件。
- 2. 将 **活动** 面板中的 Open Browser 活动拖放到 **设计器** 面板中。
- 3. 接下来，单击 Open Browser 活动的 **属性** 面板，并在 **URL** 下方键入 Environment.CurrentDirectory+"\CRM\Order Entry.html"。
- 4. 将 **Chrome** 选为 **浏览器类型** 选项：

![](img/84b06ad2832c53c5f677f8ce4c80f184_478_0.png)

图 15.7 – Open Browser 活动

- 5. 将 Maximize Window 活动拖放到 Open Browser 活动内部。这将确保订单录入 CRM 网页始终在最大化窗口中打开。

太棒了！现在您已打开订单录入 CRM 网页，让我们开始从队列读取数据。然后，我们将把记录输入到 CRM 中。

## 处理队列

在本节中，您将从队列读取事务项目，并将其输入到订单录入 CRM 网页中。

让我们从构建工作流开始：

- 1. 创建名为 Read_From_Queue 的新流程图工作流。
- 2. 接下来，拖放一个序列活动，将其重命名为获取事务项目，并将其设置为起始节点：

![](img/84b06ad2832c53c5f677f8ce4c80f184_479_0.png)

图 15.8 – 获取事务项目序列已设置为起始节点

- 3. 在获取事务项目序列内部，拖放 Try Catch 活动。
- 4. 在 `Try Catch` 活动的 **尝试** 块内部，拖放 `Get Transaction Item` 活动：

![](img/84b06ad2832c53c5f677f8ce4c80f184_480_0.png)

图 15.9 – 获取事务项目活动

- 5. 单击 **获取事务项目** 活动的 **属性** 面板，并传入以下属性：
  - `QueueName = "HenryFoodMartInvoice"`
  - `Folder Path = "Packt"`

在此示例中，Orchestrator 中的队列位于名为 `Packt` 的文件夹中。因此，要从 `Packt` 文件夹获取队列，已在 **文件夹路径** 部分中传入 `Packt`。

- 6. 向 **输出** 下的 **事务项目** 属性添加名为 `transactionItem` 的新变量：

![](img/84b06ad2832c53c5f677f8ce4c80f184_481_0.png)

图 15.10 - 属性面板

- 7. 在 Try Catch 活动的 **捕获** 块内部，捕获 `Exception` 值并添加 `Log Message` 活动。**日志级别** 属性应为 `Error`，而 **消息** 部分应为 "No queue found at the mentioned folder path"+vbLf+exception.Message+vbLf+exception.Source。
- 8. 导航到 Read_From_Queue 流程图工作流，并拖放与获取事务项目序列相连的流程决策活动。
- 9. 在流程决策活动的 **条件** 属性下键入 transactionItem IsNot Nothing：

![](img/84b06ad2832c53c5f677f8ce4c80f184_482_0.png)

图 15.11 – 用于检查事务项目的流程决策活动

此布尔条件将为您提供两种结果：当队列中存在事务项目时为 TRUE，或当没有事务项目时为 FALSE。

- 10. 拖放连接到 FALSE 分支的记录消息活动。将日志级别属性设置为 Info，并将 "No more transaction items present" 键入消息部分：

获取事务项目
从名为
HenryFoodMartInvoice
的队列获取事务项目
1 个操作
双击查看

流程决策

记录消息
记录消息以通知
没有更多事务项目可供处理
双击查看

图 15.12 – 显示队列中没有新事务项目

如果流程决策活动条件为 TRUE，您可以继续从事务项目中检索客户详细信息和发票详情。因此，让我们构建从事务项目中检索发票详细信息的流程。

### 从事务项目中检索数据

在本节中，您将从已从队列中读取的事务项目中提取 **客户详细信息** 和 **发票详情**：

- 1. 拖放一个序列活动，并将其连接到流程决策活动的 TRUE 分支。然后，将序列活动重命名为从事务中检索数据：

![](img/84b06ad2832c53c5f677f8ce4c80f184_484_0.png)

图 15.13 – 从事务中检索数据的序列

- 2. 在从事务中检索数据序列内部，拖放 Try Catch 活动。
- 3. 在 Try Catch 活动的 **尝试** 块内部，拖放一个序列活动，并将其重命名为获取发票日期。
- 4. 在获取发票日期序列内添加一个赋值活动。在 **属性** 面板下，将 `transactionItem.SpecificContent("invoiceDate").ToString` 键入 **值** 部分。接下来，在 **至** 部分中，创建名为 invoiceDate、类型为 String 的新变量：

![](img/84b06ad2832c53c5f677f8ce4c80f184_485_0.png)

图 15.14 – 获取发票日期

- 5. 在获取发票日期序列下方拖放一个序列活动，并将其重命名为获取发票编号。
- 6. 在获取发票编号序列内添加一个赋值活动。在 **属性** 面板下，将 `transactionItem.SpecificContent("invoiceNumber").ToString` 键入 **值** 部分。接下来，在 **至** 部分中，创建名为 `invoiceNumber`、类型为 `String` 的新变量。
- 7. 在获取发票编号序列下方拖放一个序列活动，并将其重命名为获取客户姓名。
- 8. 在获取客户姓名序列内添加一个赋值活动。在 **属性** 面板下，将 `transactionItem.SpecificContent("customerName").ToString` 键入 **值** 部分。接下来，在 **至** 部分中，创建名为 `customerName`、类型为 `String` 的新变量。
- 9. 在获取客户姓名序列下方拖放一个序列活动，并将其重命名为获取客户地址。
- 10. 在获取客户地址序列内添加一个赋值活动。在 **属性** 面板下，将 `transactionItem.SpecificContent("customerAddress").ToString` 键入 **值** 部分。接下来，在 **至** 部分中，创建名为 `customerAddress`、类型为 `String` 的新变量。

