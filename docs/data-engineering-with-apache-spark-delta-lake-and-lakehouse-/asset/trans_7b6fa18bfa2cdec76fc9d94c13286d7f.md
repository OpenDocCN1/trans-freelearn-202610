

我相信你会好奇这样做到底实现了什么。简单来说，就是节约云资源成本。如果一个程序意外地挂起，你是希望累积 7 天的云资源费用，还是仅仅 15–30 分钟？在这种情况下，让活动终止并单独调查意外挂起的原因可能更好。

> **重要**
> 聪明的数据工程师的标志之一是能够在坏和更坏之间做出有效选择。在某些场景下，从长远来看，选择快速失败机制会更好，因为后者可能导致大量不必要的云资源成本。

- **重试**：顾名思义，添加重试意味着在放弃之前活动将被重试的次数。Azure 数据工厂中该值的默认设置为 0，换句话说，就是只尝试活动一次。设置更高的重试值可以抵御瞬时故障，这些故障通常与网络故障和不可用性有关。在大多数情况下，值为 3 可能是一个不错的起点。然而，重试的行为会直接受到重试间隔的影响。
- **重试间隔**：重试机制会在每次重试尝试之间添加延迟。Azure 数据工厂中该值的默认值为 30 秒。如前所述，此属性经常与重试次数配对使用。

这些参数确实可以增加管道的耐久性；然而，它们并不能覆盖所有故障场景。某一天，故障总会发生。

> **重要**
> 在管道中预见并优雅地处理故障条件应得到周密规划和演练。毕竟，在数据工程师完成工作后，你将把系统移交给运营团队进行未来的监控和支持。

## 处理故障条件

如前所述，一个设计良好的管道应该能够优雅地处理故障条件。但问题是如何？让我们用以下故障场景来说明。

作为 Electroniz 管道的一部分，我们添加了从商店数据库摄取增量数据的活动。某一天，存储数据库因维护而宕机。因此，按小时触发的管道因数据库不可用而未能运行。在此场景中，Azure 数据工厂报告错误如下：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_323_0.png)

图 9.15 – 因数据库不可用导致的管道失败

总体而言，这是预期的行为。但真正的问题是如何处理这种情况？好吧，我们有几个选择——绕过失败的活动，让其余管道照常进行，或终止整个管道并单独处理故障。让我们接下来更详细地探讨这两个选项。

## 绕过失败的活动

在某些情况下，即使某个活动出现问题，也建议让其余管道继续进行。这就是操作方法：

- 1. 在 Azure 数据工厂中的每个活动内，你会注意到右下角的 **Add output icon**。点击它将显示 **Conditions** 菜单，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_324_0.png)

图 9.16 – 添加活动条件

- 2. 点击 **Failure**。现在，将 **Find SalesDB Tables** 的 **Failure** 条件链接到 **Find Currencies**。这样，如果销售数据库宕机，失败条件将不会终止整个作业；相反，它将绕过 **For Each Table** 并继续执行 **Find Currencies:**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_324_1.png)

图 9.17 – 绕过失败的活动

> **重要**
> 绕过失败活动的决定需要非常谨慎地规划。在某些场景中，后续摄取的数据可能直接依赖于失败的活动。在这种情况下，当任何活动失败时，终止整个管道会更好。

## 重新运行失败的活动

处理故障的另一种方法是，只要任何单个活动出现问题，就让整个管道失败。如果管道活动之间存在强数据依赖关系，此方法经常被优先于绕过活动的方法。在此方法中，仔细评估失败活动运行的错误消息和日志。一旦发现并修复错误，就按如下方式重新运行失败的管道：

electroniz_batch_ingestion_pipeline

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_325_0.png)

图 9.18 – 重新运行失败的活动

以下列表演示了活动：

- 1. **重新运行**：整个管道可以使用重新运行选项重新运行。由于某些活动可能运行多次，因此此方法存在缺点，因为它可能在青铜层中重复数据。
- 2. **从失败的活动重新运行**：或者，如果使用此选项，你将只从失败的活动开始运行。虽然此方法可以避免重复，但它倾向于摄取不一致的数据，因为部分数据可能在不同的时间间隔内被摄取。

> **重要**
> 重新运行整个管道与从失败活动运行之间的决定需要仔细规划并充分测试。数据工程师应就所选方法征求客户的批准。

## 添加警报功能

在前一节中，我们仔细规划并执行了有关管道故障和重启机制的方法。为了增加管道的耐久性，我们应配置一些警报机制。如果没有适当的警报机制，管理员用户可能无法及时做出反应并从故障中恢复。这可能是灾难性的，因为从管道故障中恢复的延迟已被证明会导致巨大的客户不满、实时决策延迟，甚至更糟——数据损坏：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_326_0.png)

图 9.19 – 为管道添加警报

以下是创建实时警报的步骤：

- 1. 咨询客户，确定应接收警报的用户。电子邮件和短信是两种常见选择。与其向个人电子邮件地址发送警报，强烈建议你将警报发送到电子邮件组，例如 administrators@site.com。
- 2. 在 Azure 数据工厂中，点击 Monitor。现在点击 Alerts and metrics。然后，点击 New alert rule:
  - Alert rule name: Error Alerts
  - Description: Alert for pipeline failures
  - Severity: Sev1

- 3. 点击 + Add criteria。从 Add criteria 屏幕中，选择 Failed pipeline runs metrics:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_327_0.png)

图 9.20 – 为管道失败添加新警报

- 4. 点击 Continue:
  - 导航到 Configure alert logic 屏幕。
  - 在 Dimension 下方，输入以下内容：
  - Name: 点击 Select all

- **FailureType:** 点击 **Select all:**

# Configure alert logic

Failed pipeline runs metrics

Show history

Over the last 6 hours

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_328_0.png)

选择维度值将帮助你筛选到正确的时间序列。

| Dimension | Values |
| Name | 5 selected |
| FailureType | 3 selected |

# Alert logic

Condition *

Greater than

Time aggregation *

Total

Threshold count *

0

Update criteria

Cancel

图 9.21 – 配置警报逻辑

- 5. 点击 **Update criteria**。
- 现在，点击 + **Configure notification**。选择 **Create new**:

# Configure notification

Notify your team via email and text messages or automate actions using webhooks, runbooks, functions logic apps or integrating with external ITSM solutions.

- Create new
- Use existing

Action group name *
Pipeline Administrators

Short name *
PADMIN

Notifications *

+ Add notification

图 9.22 – 配置通知

- 6. 点击 + **Add notification**。

- 7. 点击 **Email** 复选框并添加警报组的电子邮件 ID。

