

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_355_0.png)

Figure 10.23 – State of data sharing currently

不幸的是，这些数据共享方法存在若干问题：

- **Complex**: 这些数据共享机制设置和使用起来可能很复杂，因为它们可能需要交换密钥/密码并使用多种不同的工具。
- **Insecure**: 这些机制可能不安全，无法保护静态数据或传输中数据的安全。这意味着经典的 **man-in-the-middle** 攻击可能会以明文形式泄露数据。
- **Tracking**: 目前没有有效的方法来追踪谁与谁共享了数据以及使用的是哪种机制。
- **Control**: 你无法控制共享数据的使用者。
- **Updates**: 数据共享是一个持续的过程，而前面提到的机制并不能提供简单的方式来共享更新。

幸运的是，有一种有效的替代方案。Azure 允许组织使用名为 **Azure Data Share** 的简单且安全的服务在内部和外部共享数据。使用 Azure Data Share，共享数据的用户（**providers**）可以轻松邀请他们希望共享数据的用户（**consumers**）。在数据与消费者共享之前，他们需要接受数据使用条款。数据更新也可以按照提供方配置的设定频率进行共享。数据共享可以以两种方式进行。我们将在以下小节中分别介绍这两种方式。

## In-place sharing

在这种情况下，数据始终保留在提供方的 Azure 订阅中，并向消费者的 Azure 订阅发送一个符号链接。消费者可以使用此符号链接读取数据，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_356_0.png)

Figure 10.24 – In-place sharing using Azure Data Share

## Snapshot sharing

在这种情况下，数据从数据提供方的 Azure 订阅移动并落地到数据消费方的 Azure 订阅中：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_356_1.png)

Figure 10.25 - Snapshot sharing using Azure Data Share

让我们通过一个简单的练习来了解更多关于共享数据的信息。对于此练习，你需要一位共享伙伴来充当消费者。你的伙伴应拥有一个可操作的 Azure 账户。如果没有，他们可以创建一个新的。

## Preparing the Azure resources

我们将开始准备共享数据所需的 Azure 资源：

- 1. 在 Azure 门户中，搜索 Data Shares 并导航到它。
- 2. 在 Data Shares 页面，点击 Create。
- 3. 在 Create Data Share 页面，输入以下详细信息：
  - Subscription: Free Tier or Paid Subscription
  - Resource group: training_rg
  - Location: East US 2
  - Name: trainingshare
点击 Review + create：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_357_0.png)

Figure 10.26 – Creating a new data share

- 4. 注意顶部出现的 Validation Passed 消息。按下 Create。
- 5. 新创建的数据共享现在将被部署。部署完成后，你应收到一条 Your deployment is complete 的消息。

6. 点击 Go to resource。这应会带你进入新创建的数据共享页面：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_358_0.png)

Figure 10.27 – Home screen of the new data share

> **Important**
> 决定在内部或外部共享数据的决定极其敏感。作为数据工程师，你在进行数据共享之前必须强制寻求客户的书面批准。

## Creating a data share

现在我们已经创建了一个新的数据共享账户，可以继续共享数据：

- 1. 点击 Start sharing your data。现在，点击 Create 并提供以下值：
  - Share name: golddata
  - Share type: Snapshot

点击 Continue。这应会带你进入 Datasets 选项卡。从这里，点击 Add datasets 并选择 Azure Data Lake Gen2：

Figure 10.28 – Choosing a dataset type for the data share

2. 点击 Next 并提供以下值：
  - **Subscriptions:** Free Tier or Paid Subscription
  - **Resource groups:** training_rg
  - **Storage accounts:** traininglakehouse:

Figure 10.29 – Choosing an Azure Data Lake Storage account for the data share

3. 点击 Next。

在 Select datasets 页面，选择 gold 并点击 Next：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_360_0.png)

Figure 10.30 – Choosing the gold dataset for the data share

4. 在 Rename datasets 页面，保持名称不变或按需更改：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_360_1.png)

Figure 10.31 – Renaming the dataset for the data share

5. 点击 Add datasets 然后 Continue。

现在，我们将添加将接收共享的消费者（伙伴）的电子邮件地址值。

6. 点击 **Add recipient**。输入收件人的电子邮件地址并点击 **Add**，然后 **Send invitation**：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_361_0.png)

Figure 10.32 – Adding the email address of the consumer

7. Azure 现在将向消费者发送电子邮件，消费者需要接受邀请并执行一系列步骤以从新创建的数据共享中下载数据。

请注意，接下来的步骤系列是从消费者的角度出发的。此后显示的一些屏幕截图将反映消费者在收到电子邮件后所看到的内容。为了显示这种差异，所有具有 **白色背景** 的屏幕截图均来自消费者的 Azure 账户。

8. 消费者将收到如下所示的电子邮件：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_361_1.png)

Figure 10.33 – Invitation email for the consumer

9. 点击 View Invitation 后，请求将被转发到消费者的 Azure 门户。登录门户后，将出现以下屏幕：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_362_0.png)

Figure 10.34 – The customer viewing the invitation

点击 golddata 并提供以下值：
  - Data share account name: golddatashare
  - Subscription: Free Tier or Paid Subscription
  - Resource group: training_rg
  - Location: East US 2

点击 Create。你将收到以下错误：

The subscription is not registered to use namespace 'Microsoft.DataShare'. See https://aka.ms/rps-not-found for how to register subscriptions.

无需惊慌 – Azure 订阅默认不会启用数据共享。

10. 导航到 Subscriptions 页面并点击你的 Azure 订阅：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_363_0.png)

Figure 10.35 – Enabling data sharing in the consumer's subscription

11. 现在，使用左侧菜单点击 Resource providers：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_363_1.png)

Figure 10.36 – Navigating to the resource providers in the consumer's subscription

12. 使用顶部的搜索栏，输入 datashare。注意 Microsoft.Datashare 目前尚未注册：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_364_0.png)

Figure 10.37 – Checking the state of Microsoft.Datashare in the consumer's subscription

13. 选择 Microsoft.Datashare 并点击 Register。Microsoft.Datashare 现在将为消费者的 Azure 订阅注册。请注意，此步骤可能需要几分钟才能完成：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_365_0.png)

Figure 10.38 – Registering Microsoft.Datashare in the consumer's subscription

14. 一旦状态更改为 Registered，返回数据共享账户选项卡并再次点击 Create。希望这次过程将不会出现任何错误：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_365_1.png)

Figure 10.39 – Creating a new data share in the consumer's subscription

15. 数据共享邀请现在将被显示。输入以下屏幕截图中显示的详细信息，然后点击 Accept and configure：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_366_0.png)

Figure 10.40 – Setting the data share parameters in the consumer's subscription

16. 成功数据共享的确认屏幕应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_366_1.png)

Figure 10.41 – Home screen of the data share in the consumer's subscription

