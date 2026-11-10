
## 20 测试自定义工作流

一旦您创建、验证、安装并在 Web Publisher 中使自定义工作流模板可用后，您可以通过从自定义工作流模板创建的工作流实例发送内容来测试它。

图 20.1 描述了我们在上一章中创建的自定义工作流中的活动序列。在测试工作流步骤时，这应作为很好的帮助。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_332_0.png)

图 20.1：自定义工作流模板

然而，您首先需要创建将参与工作流的 Web Publisher 用户。在我们的例子中，在开始使用和测试工作流之前，我们需要在 Docbase 中创建一个内容作者用户和一个内容管理员用户。

### 20.1 在 Documentum 中为工作流创建用户

以下步骤将解释如何在 Documentum 中创建用户：

- 1. 以管理员或超级用户身份登录 Documentum Administrator 并转到 User Management | Users。
- 选择菜单选项 File | New | User 以打开 New User 创建屏幕。
- 2. 填写创建内容作者用户所需的详细信息，如图 20.2 所示。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_333_0.png)

图 20.2：创建内容作者用户

User OS 名称 gauravk 已存在于 Windows 域中，并由 Documentum 用于操作系统身份验证目的。因此，您无需在图 20.2 所示的用户创建屏幕中为 NT 用户提供密码。但是，您需要根据自己的环境指定用户设置/参数。

同样，根据图 20.3 所示的详细信息创建一个内容管理员用户。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_334_0.png)

图 20.3：创建内容管理员用户

有关用户、组及其属性的详细信息，请参阅 Documentum Content Server Fundamentals 和 Administration 手册。

### 20.2 将内容提交到自定义工作流

在测试工作流时，请将自定义工作流模板可视化图表（请参阅图 20.1）放在面前。这将帮助您了解当前正在执行哪个特定活动、谁在执行该活动以及下一个活动是什么。

下面提到的简单步骤应遵循以测试我们在上一章中创建的 Custom_workflow。

- 1. 以内容作者用户身份（例如：gauravk）登录 Web Publisher 并通过 News Article 模板创建一些 News Article 内容。
- 请参阅图 20.4；选中新创建内容的复选框并单击 Start Workflow 图标（或选择菜单选项 Tools | Workflow | Start）以将内容提交到工作流实例。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_335_0.png)

图 20.4：将内容提交到工作流实例

- 2. 从 Docbase 中所有已安装和可用的工作流模板列表中选择 Custom_Workflow 工作流模板，如图 20.5 所示。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_335_1.png)

图 20.5：选择工作流模板

单击 OK。

- 3. 如图 20.6 所示，为工作流实例提供简要描述以通过其名称（例如：workflow_Testing）识别它。

您还可以为工作流任务选择优先级并为路由内容（在 Documentum 中称为 change set）提供生效日期。请注意，change set 的生效日期会覆盖路由内容的现有生效日期。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_336_0.png)

图 20.6：为工作流实例提供基本信息

单击 Finish 以将内容提交到工作流，或单击 Next 以在最终提交到工作流之前向下一个活动/任务的接收者提供一些说明/注释。

- 4. 以内容管理员用户身份登录 Web Publisher 并转到 Web Publisher 左侧面板中显示的 Inbox。
您将在内容管理员的 Inbox 中注意到已提交的工作流任务（请参阅图 20.7）。Docbase 中的所有内容管理员用户都会收到此特定任务。
请注意，任务由活动名称和工作流名称标识（在我们的例子中为 Review_Act : Workflow_Testing）。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_336_1.png)

图 20.7：内容管理员审阅的工作流任务

此时任务处于 Dormant 状态，如图 20.7 的 Status 列所示。

单击任务链接 Review_Act : Workflow_Testing。

- 5. Task Manager: Info 屏幕打开，显示路由内容和执行者（本例中为内容管理员）的说明。

请参阅图 20.8；请注意，在到达内容管理员的 Inbox 之前，内容已通过自动活动 PromoteStaging_Act 提升到 Staging 生命周期状态。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_337_0.png)

图 20.8：内容管理员 Inbox 中的任务信息屏幕

选中内容文件的复选框并单击 Accept 以接受此工作流任务。这会将此任务从所有其他内容管理员用户的 Inbox 中移除，并将任务标记为 Acquired 状态。

- 6. 此时，内容处于 Review_Act 活动中，内容管理员可以向前流程提交以批准它，或向后流程拒绝以将其降级到 WIP 状态以供内容作者进行更正。

正如在图 20.9 中可以看到的那样，Submit 和 Reject 两个按钮正是为此目的而启用的。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_338_0.png)

图 20.9：已接受的工作流任务

内容管理员可以通过选中内容文件的复选框并选择菜单选项 View | Properties | Info 来查看内容的属性。

要查看内容文件的实际网页内容，内容管理员可以通过选择菜单选项 View | Web View 来执行网页查看。

（请参阅第 22 章：Web Viewing Content Files，了解 Web View 的详细信息及其用法）。

- 7. 假设内容管理员对内容和/或其属性不满意，并希望进行一些更正。
转到图 20.9 中显示的 Comments 选项卡并为内容作者提供评论。单击 Reject 将其退回给内容作者用户。
此时，系统会调用自动活动 DemoteWIP_Act 将内容降级回 WIP 生命周期状态。
- 8. 以内容作者用户身份登录 Web Publisher 并转到 Inbox。
被拒绝的任务（ContentCorrection_Act : Workflow_Testing）显示在所有内容作者用户的 Inbox 中，如图 20.10 所示。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_339_0.png)

图 20.10：供内容作者拒绝的任务

单击任务链接以打开 Task Manager: Info 屏幕（请参阅图 20.11）。查看审阅者要求更正的评论。

单击 Accept 按钮以接受此工作流任务。正如我们之前看到的那样，这会将任务从所有其他内容作者用户的 Inbox 中移除并将任务标记为 Acquired 状态。

- 9. 内容作者可以选中内容文件名旁边的复选框并选择菜单选项 View | Properties | Info 以更正任何内容属性/属性。

此外，内容作者可以选择菜单选项 File | Edit 以在 Web Publisher 中打开内容以修改任何内容字段。

进行必要的更正后，选择菜单选项 File | Check In 将修改后的内容版本检入 Docbase。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_339_1.png)

图 20.11：内容作者 Inbox 中的 Task Manager: Info 屏幕

- 10. 单击 Submit 再次将更正后的内容提交给审阅者。

任务完成屏幕显示在图 20.12 中。单击 Ok，以便系统将此任务从内容作者的 Inbox 中移除。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_340_0.png)

图 20.12：任务完成屏幕

请注意，在此阶段，系统会调用自动活动 PromoteStaging_Act 将内容提升到 Staging 生命周期状态。

- 11. 以内容管理员用户身份登录 Web Publisher 并重复上面描述的步骤 4 到 6。一旦您对内容作者所做的更正感到满意，请选择内容文件并在 Task Manager: Info 屏幕中单击 Submit。

提交内容会通过 PromoteApproved_Act 自动活动自动将内容提升到 Approved 状态。

根据内容文件的生效日期，Documentum Monitor_Lifecycles Job 会将内容转为 Active。此时，内容已完成所有工作流步骤，可以重新提交到新的工作流实例！

这涵盖了测试自定义工作流模板所需的必要步骤。显然，您的自定义工作流模板越复杂，其测试程序就越复杂。

继续使用我们在上一章中创建的自定义工作流模板进行操作。相应地，您可以测试工作流中的所有可能场景，例如特定任务的多个执行者、通知计时器以及工作流中路由的多个包。
