

8. 在 **属性** 面板下，单击 **ItemInformation** 属性的省略号图标：

| 属性 | 值 |
|---|---|
| 截止日期 | 队列...之前的日期 |
| ItemInformation | (集合) |
| ItemInformationCollection | 用于添加...的集合变量 |
| 推迟日期 | 队列...之后的日期 |
| 优先级 | 正常 |
| 队列名称 | "HenryFoodMartInvoice" |

图 14.45 – ItemInformation 集合属性

9. 在 **ItemInformation** 窗口中，创建名为 invoiceDate、invoiceNumber、customerName、customerAddress、customerPhoneNo、customerEmail 和 invoiceParticulars 的七个 String 类型参数。

10. 让我们为所有参数赋值：

- invoiceDate = in_invoiceDate.Split(":", StringSplitOptions.None)(1).ToString.Trim
- invoiceNumber = in_invoiceNumber.Split("#", StringSplitOptions.None)(1).ToString.Trim
- customerName = in_customerName
- customerAddress = in_customerAddress
- customerPhoneNo = in_customerPhoneNo
- customerEmail = in_customerEmail
- invoiceParticulars = JsonConvert.SerializeObject(in_dt_invoiceParticulars)

在 **值** 部分为所有值赋值后，您的 **ItemInformation** 窗口应如下所示：

| 名称 | 方向 | 类型 | 值 |
|---|---|---|---|
| invoiceDate | In | String | in_invoiceDate.Split(":", StringSplitOptions.None)(1).ToString.Trim |
| invoiceNumber | In | String | in_invoiceNumber.Split("#", StringSplitOptions.None)(1).ToString.Trim |
| customerName | In | String | in_customerName |
| customerAddress | In | String | in_customerAddress |
| customerPhoneNo | In | String | in_customerPhoneNo |
| customerEmail | In | String | in_customerEmail |
| invoiceParticulars | In | String | JsonConvert.SerializeObject(in_dt_invoiceParticulars) |

图 14.46 – 已分配所有值的 ItemInformation 窗口

11. 发票日期和发票号中包含额外字符。要去除这些字符，请使用带有适当分隔符的 Split() 函数。如前所述，DataTable 参数在上传到队列之前必须序列化。JsonConvert 类的 SerializeObject() 函数可帮助将其序列化为 JSON 对象。

12. 最后，在 Catches 块中，捕获 SystemException。

13. 另外，记录一条消息，日志级别为 Error，消息为 "Unable to Upload to Queue" + vbLf + exception.Message + vbLf + exception.Source。

14. 返回到 Read_Invoice 序列，从 **项目** 面板将 Upload_To_Queue.xaml 工作流拖放到 For Each 活动主体中 Invoke_Extract_Invoice_Particular 工作流活动的下方：

![](img/84b06ad2832c53c5f677f8ce4c80f184_466_0.png)

图 14.47 – 调用 Upload_to_Queue.xaml

15. 单击 **导入参数** 按钮，为所有七个 In 参数传递变量，如下图所示：

| 名称 | 方向 | 类型 | 值 |
|---|---|---|---|
| in_invoiceDate | In | String | invoiceDate |
| in_invoiceNumber | In | String | invoiceNumber |
| in_customerName | In | String | customerName |
| in_customerAddress | In | String | customerAddress |
| in_customerPhoneNo | In | String | customerPhoneNo |
| in_customerEmail | In | String | customerEmail |
| in_dt_invoiceParticulars | In | DataTable | dt_invoiceParticulars |

图 14.48 – 导入参数

现在，您已完成上传到队列部分，是时候组装 Main.xaml 文件以运行流程了。

## 组装 Main.xaml

是时候运行流程了，但在运行流程之前，让我们先在 Main.xaml 文件中调用 Download_Invoice.xaml 和 Read_Invoice.xaml 工作流：

1. 在 Main.xaml 工作流中，创建名为 isMail 的布尔类型变量。然后，从 **项目** 面板将 Download_Invoice.xaml 工作流拖放到 Main.xaml 工作流中。

2. 单击 **导入参数** 按钮，将 isMail 变量作为值传递给 out_isMail 参数：

![](img/84b06ad2832c53c5f677f8ce4c80f184_468_0.png)

| 名称 | 方向 | 类型 | 值 |
|---|---|---|---|
| out_isMail | Out | Boolean | isMail |
| 创建参数 | | | |

图 14.49 – 调用 Download_Invoice 工作流

3. 接下来，拖放 **如果** 活动，并在 **条件** 中传递 **isMail** 变量。如果 **isMail** 变量的值为 **真**，则可以开始读取发票；否则，将停止流程。

4. 从 **项目** 面板将 **Read_Invoice.xaml** 工作流拖放到 **如果** 活动的 **然后** 块中。

5. 在其下方，添加 **记录消息** 活动，日志级别设为 **信息**，并将消息设置为 "All invoices details uploaded to queue"。

6. 在 **否则** 块中，添加 **信息** 级别的 **记录消息** 活动，并将消息设置为 "No invoice to process"：

![](img/84b06ad2832c53c5f677f8ce4c80f184_469_0.png)

图 14.50 – Main.xaml 工作流

7. 现在，通过单击 **设计** 功能区选项卡上的 **运行** 按钮运行整个流程。流程在没有任何错误的情况下完成，所有发票详细信息都已上传到 Orchestrator 队列：

![](img/84b06ad2832c53c5f677f8ce4c80f184_470_0.png)

图 14.51 – 队列中的事务项目

恭喜！您已成功构建发票处理分派器模型的自动化。现在，您可以将代码发布到 Orchestrator，以便从 Orchestrator 页面或 UiPath Assistant 运行机器人。

## 摘要

在本章中，您开发了一个从发票中提取相关数据以供进一步处理的自动化流程。首先，机器人阅读电子邮件并下载以电子邮件附件形式以 PDF 接收的发票。然后，它从发票中提取特定数据并将其上传到 Orchestrator 队列。

在下一章中，您将学习如何构建执行者模型，以从 Orchestrator 队列中获取值并将其输入到订单录入 CRM 中。

## 15

## 发票处理 - 执行者

在 Henry's Food Mart，财务和会计部门已选择发票处理流程进行自动化。发票处理流程阅读电子邮件并下载以电子邮件附件形式以 PDF 接收的发票。然后，它从发票中提取客户信息和发票详细信息，并将其输入到订单录入 CRM 网页中。

您通过遵循分派器-执行者模型开始构建此流程。在第 14 章“发票处理 – 分派器”中，您学习了如何构建一个从电子邮件下载未读发票、从每张发票中提取客户详细信息和发票详细信息，然后将其上传到名为 HenryFoodMartInvoice 的 Orchestrator 队列的自动化流程。

在本章中，您将构建执行者模型，以将发票数据输入到订单录入 CRM 网页中。

以下是您将在本发票处理 – 执行者项目中学习的内容列表：

- 浏览器自动化
- 使用不同类型的工作流
- 使用 Orchestrator 队列
- 修改选择器
- 在 UiPath Studio 中记录日志
- 处理异常
- 反序列化 JSON
- 使用 DataTables
- 更新队列状态

### 技术要求

让我们了解此项目所需的要求：

- UiPath Studio Community/Enterprise/Academic Alliance Edition 20.10 及更高版本，且必须连接到 UiPath Orchestrator 服务。
- 访问 UiPath Orchestrator 服务
- 订单录入 CRM 网页
- Google Chrome 浏览器
- 互联网访问权限

您可以从 GitHub 上的 https://github.com/PacktPublishing/UiPath-Associate-Certification-Guide/tree/main/Chapter15/Solution 获取发票处理分派器的完整代码。

### 项目概览

在本章中，您将设计发票处理流程的执行者模型。让我们详细了解此过程。以下是您应该执行的步骤：

1. 编写机器人以启动订单录入 CRM 网页。
2. 编写机器人以从 Orchestrator 工作流中名为 **HenryFoodMartInvoice** 的队列读取事务项目。
3. 编写机器人以从从队列中读取的事务项目中提取以下相关字段：
    - **客户详细信息：**
        - 客户姓名
        - 客户地址
        - 客户电话号码
        - 客户电子邮件 ID
    - **发票详细信息：**
        - 发票日期
        - 发票号
    - **订单信息：**
        - 项目数量
        - 项目描述
        - 项目单价
        - 行总金额
        - 小计金额
        - 运费金额
        - 总金额

