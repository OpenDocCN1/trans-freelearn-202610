

## 摘要

在本章中，您了解了什么是调试以及它如何帮助排查并从进程中移除 bug。首先，您看到了 DEBUG 功能区条中存在的各种调试操作菜单，这些菜单可帮助您导航以对单个文件或整个项目进行调试。接着，您了解了多个调试面板，它们可帮助您修复错误。您还了解了在流程开发的构建阶段内，UiPath Studio 中的测试活动。最后，您使用之前学到的所有技能调试了一个工作流以修复 bug。

在下一章中，您将学习如何使用分发器和执行器的概念构建自动化项目。

## 发票处理 - 分发器

**亨利食品超市** 是美国的一家连锁便利店，总部位于纽约，目前在美国有 400 多家门店。**发票处理** 流程已被选为 RPA 自动化，作为亨利食品超市财务和会计部门内更大规模项目计划的一部分。

此流程自动化的目标与项目商业案例相关，主要旨在完成以下任务：

- 加快发票处理速度。
- 减少发票处理中的冗余活动。
- 通过消除错误来提升整体性能和可靠性。

发票处理流程会读取电子邮件，并下载以电子邮件附件形式接收的 PDF 发票。然后，它从发票中提取客户信息和发票详情，并将其录入到 **订单录入 CRM** 中。

您将分两部分构建此流程。您将遵循 **分发器-执行器** 模型并创建两个不同的流程。在本章中，您将构建分发器模型，在下一章中，您将构建执行器模型。

作为 *发票处理 – 分发器* 项目的一部分，您将学习以下内容：

- 创建和使用 Orchestrator 资产
- 使用邮件自动化
- 在 UiPath Studio 中操作数据
- 自动化 PDF 文件以读取数据
- 修改选择器
- 使用正则表达式
- 使用 UiPath Studio 数据抓取向导
- 创建并上传数据到 Orchestrator 队列
- UiPath Studio 中的异常处理
- UiPath Studio 中的日志记录

## 技术要求

让我们了解本项目的要求：

- UiPath Studio Community/Enterprise/Academic Alliance Edition 版本 20.10 及以上，已连接到 UiPath Orchestrator
- 访问 UiPath Orchestrator 服务
- 一个 Gmail 账户
- Adobe Acrobat PDF Reader
- 访问互联网
- 在安装了 UiPath Studio 的计算机上具有读写权限

您可以在 GitHub 上获取发票处理分发器的完整代码：`https://github.com/PacktPublishing/UiPath-Associate-Certification-Guide.git`。

## 项目概述

在本章中，您将设计发票处理的分发器模型。让我们详细了解流程。以下是您需要执行的步骤：

- 1. 编程一个机器人，以从您的 Gmail 账户中读取最后一封未读邮件。
- 2. 编程一个机器人，以下载已读邮件中的所有发票附件。下载附件的条件如下：
  - 邮件主题行必须为 Invoice-Henry-FM-Invoice Number-Customer Name。
  - 附件的命名规范必须为 Invoice-Henry FM-Invoice Number-Customer Name.pdf。
- 3. 编程一个机器人，将所有发票附件保存在项目目录中名为 Invoice 的文件夹中。
- 4. 编程一个机器人，从发票中提取以下相关字段：
  - 客户详细信息：
    - 客户姓名
    - 客户地址
    - 客户电话号码
    - 客户电子邮件 ID
  - 发票详情：
    - 发票日期
    - 发票编号
  - 订单信息：
    - 商品数量
    - 商品描述
    - 商品单价
    - 行总金额
  - 小计金额
  - 运费金额
  - 总金额
- 5. 编程一个机器人，将以下详细信息上传到 Orchestrator 队列：
  - 客户详细信息
  - 发票详情
- 6. 对剩余发票重复步骤 4–5。
- 7. 编程机器人，如果步骤 2 中从邮件下载的所有附件都已处理，则成功结束流程。

让我们借助流程图来了解整个流程：

![](img/84b06ad2832c53c5f677f8ce4c80f184_432_0.png)

图 14.1 – 高级流程图

在开始设计流程自动化之前，请从此 GitHub 仓库的 Input File – Invoice 文件夹下载所有提供的发票：https://github.com/PacktPublishing/UiPath-Associate-Certification-Guide/tree/main/Chapter14/Input%20File%20%E2%80%93%20Invoice。

从您自己的 Gmail 账户为每张单独的发票发送一封单独的邮件，机器人将从此处读取邮件。主题行必须为 Invoice-Henry-FM-Invoice Number-Customer Name（发票编号和客户姓名出现在发票文件名中）– 例如，如果客户姓名是 Adam Smith，发票编号是 8745，那么邮件的主题行应为 Invoice-Henry-FM-8745-Adam Smith。

以下是一些示例：

![](img/84b06ad2832c53c5f677f8ce4c80f184_433_0.png)

图 14.2 – 显示主题行和附件的 Gmail 收件箱片段

现在您已将发票发送到您的邮件 ID，让我们开始构建自动化。

## 设置项目

在开始构建流程之前，让我们先正确设置所有 UiPath Studio 配置：

- 1. 打开 UiPath Studio，创建名为 Invoice_Processing_Dispatcher 的新流程：

![](img/84b06ad2832c53c5f677f8ce4c80f184_433_1.png)

图 14.3 – 创建新流程

- 2. 单击设计器功能区条上的管理程序包，并将项目依赖项更新为以下版本：
  - UiPath.Excel.Activities – v2.10.4
  - UiPath.Mail.Activities – v1.10.5
  - UiPath.System.Activities – v21.4.1
  - UiPath.UIAutomation.Activities – v21.4.4

![](img/84b06ad2832c53c5f677f8ce4c80f184_434_0.png)

图 14.4 – 更新依赖项

- 3. 现在您已将依赖项更新为正确版本，让我们再安装一个依赖项，UiPath.PDF.Activities – v3.4.0：

![](img/84b06ad2832c53c5f677f8ce4c80f184_435_0.png)

图 14.5 – 已添加 PDF 依赖项

- 4. 在 Orchestrator 网页的您的租户下，导航到 **文件夹** 部分并创建名为 Packt 的新文件夹。

现在您已配置好所需的一切，让我们开始构建流程。

## 下载发票

在本节中，您将设计机器人以从 Gmail 账户下载所有发票。以下流程图说明了下载发票的高级流程：

![](img/84b06ad2832c53c5f677f8ce4c80f184_436_0.png)

图 14.6 – 下载发票

下载发票是整个流程的第一步。让我们构建它：

- 1. 单击 **设计** 功能区条上的 **新建** 按钮，并选择 **序列**。
- 2. 将序列命名为 `Download_Invoice` 并单击 **创建** 按钮：

![](img/84b06ad2832c53c5f677f8ce4c80f184_436_1.png)

图 14.7 – 创建 Download_Invoice 序列

- 3. 登录到您的 `cloud.uipath.com` 账户并启动 Orchestrator 服务。在 **资产** 部分下，创建一个名为 `Mail Credential` 的资产，并为 **类型** 选择 **凭据**。接着，输入您的 Gmail ID **用户名** 和 **密码** 详细信息，然后单击 **创建** 按钮：

资产 / 添加资产

**资产名称** `Mail Credential` **类型** `Credential`

**描述** `Gmail email id and password`

**凭据存储** `Orchestrator Database`

**全局值**
**用户名** `@gmail.com` **密码** `********`

启用全局值后，每个用户都将收到它，除非在下表中特别替换。

图 14.8 - 创建凭据资产以存储 Gmail 凭据

在此场景中，**Mail Credential** 资产是在个人工作区文件夹中创建的。如果您愿意，可以在 `Packt` 文件夹中创建该资产。

