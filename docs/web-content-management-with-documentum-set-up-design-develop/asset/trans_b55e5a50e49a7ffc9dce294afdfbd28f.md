
![](img/b18244e6621ecc39cef03ff6c0e55e7c_323_0.png)

图 19.20：活动检查器通知选项卡

6. 转换选项卡（参见图 19.21）允许您指定工作流中哪些活动接下来跟随此特定活动。如果此活动有多個连接的活动（输出流），您可以：
- 指定系统在该活动完成后选择所有连接的活动。
- 允许活动执行者在运行时选择该活动完成后可以选中的下一个活动的数量。
- 指定某些条件，基于这些条件可在该活动完成后选择下一个活动。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_324_0.png)

图 19.21：活动检查器中转换选项卡的选项

参见图 19.22；如果活动有多个执行者（基于执行者选项卡下的设置），您可以指定活动是在所有执行者完成其任务时结束，还是在一定数量的执行者完成其任务项时结束。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_324_1.png)

图 19.22：活动检查器转换选项卡

请参阅图 19.6 并注意 Review_Act 活动有两个输出流：一个正向流将其引导至 PromoteApproved_Act 活动，以及一个向后/拒绝流将其引导至 DemoteWIP_Act 活动。

在这种情况下，从下拉菜单中选择“让活动的执行者选择”选项，允许活动的执行者选择多达两个下一个活动（参见图 19.23）。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_325_0.png)

图 19.23：Transition tab setting for the Review_Act activity

7. 显示选项卡（参见图 19.24）只需允许您指定活动在工作流模板编辑器中视觉上如何显示。您可以通过显示选项卡控制以下内容：
- 用于在工作流模板编辑器中显示活动的图像及其显示尺寸。
- 用于在工作流模板编辑器中显示活动标签的字体、字体大小和其他格式功能。
- 是否应在工作流模板编辑器中将活动名称或活动执行者显示为活动的标签。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_326_0.png)

图 19.24：活动检查器显示选项卡

单击“应用”然后“确定”以完成 Start_Act 活动的设置。参阅图 19.6 和以下各节中的表格以创建自定义工作流模板的其他活动。

## 19.3.1 活动的执行者选项卡

| 活动名称 | 手动/自动 | 执行者 | 方法 | 保存执行结果 | 方法超时（秒） | 方法失败时 |
|---|---|---|---|---|---|---|
| Start_Act | Auto | Docbase owner | wcmInitializeProcessWorkflow | Yes | 300 | Stop |
| PromoteStaging_Act | Auto | Docbase owner | wcmPromoteToStaging | Yes | 300 | Stop |
| Review_Act | Manual | Single user from group: content manager | N/A | N/A | N/A | N/A |
| DemoteWIP_Act | Auto | Docbase owner | wcmDemoteToWIP | Yes | 300 | Stop |
| ContentCorrection_Act | Manual | Single user from group: content author | N/A | N/A | N/A | N/A |
| PromoteApproved_Act | Auto | Docbase owner | WcmPromoteToApproved | Yes | 300 | Stop |
| End_Act | Auto | Docbase owner | WcmTerminateProcessWorkflow | Yes | 300 | Stop |

图 19.25：执行者选项卡设置

## 19.3.2 活动的触发选项卡

| 活动名称 | 活动触发时 | 是否可多次运行？ |
|---|---|---|
| Start_Act | All input flows selected | Yes |
| PromoteStaging_Act | 1 out of 2 possible input flows | Yes |
| Review_Act | All input flows selected | Yes |
| DemoteWIP_Act | All input flows selected | Yes |
| ContentCorrection_Act | All input flows selected | Yes |
| PromoteApproved_Act | All input flows selected | Yes |
| End_Act | All input flows selected | Yes |

图 19.26：触发选项卡设置

## 19.3.3 活动的通知选项卡

| 活动名称 | 活动未在...内触发 | 活动工作未在...后完成 |
|---|---|---|
| Start_Act | 1 hour | 1 hour |
| PromoteStaging_Act | 1 hour | 1 hour |
| Review_Act | 1 hour | 1 hour |
| DemoteWIP_Act | 1 hour | 1 hour |
| ContentCorrection_Act | 1 hour | 1 hour |
| PromoteApproved_Act | 1 hour | 1 hour |
| End_Act | 1 hour | 1 hour |

图 19.27：通知选项卡设置

## 19.3.4 活动的转换选项卡

| 活动名称 | 如何选择下一个活动？ | 何时完成活动 | 如果同时选择拒绝和正向活动 |
|---|---|---|---|
| Start_Act | Select all connected activities | All performers complete | N/A |
| PromoteStaging_Act | Select all connected activities | All performers complete | N/A |
| Review_Act | Let the activity's performer choose (select up to 2 next activities) | All performers complete | Start all selected activities |
| DemoteWIP_Act | Select all connected activities | All performers complete | N/A |
| ContentCorrection_Act | Select all connected activities | All performers complete | N/A |
| PromoteApproved_Act | Select all connected activities | All performers complete | N/A |
| End_Act | Select all connected activities | All performers complete | N/A |

图 19.28：转换选项卡设置

## 19.4 向连接流添加包

双击特定连接流或选择连接流并选择菜单选项 Tools | Flow Inspector 以打开如图 19.29 所示的流检查器屏幕。

单击“添加新包”按钮并提供路由包的详细信息，例如其名称和对象类型，以及路由内容的版本。

您可以为包提供有意义的名称，或选择 Documentum 提供的默认包命名约定（Package0、Package1 等）。

包的默认版本标签为 CURRENT，这确保包将包含工作流中最新的内容版本。

您甚至可以为包中的路由内容指定符号（区分大小写）标签，例如 WIP，或数字版本标签，例如 1.0。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_329_0.png)

图 19.29：通过流检查器屏幕添加包

请记住，您必须对工作流模板中连接各种活动的每个流重复这些步骤。

## Documentum 5.3 更新

Documentum 版本 5.3 通过提供对单个收件箱的支持引入了新功能，该收件箱可包含来自多个 Docbase 仓库的多个任务。

恭喜！您已创建自己的自定义工作流模板，并已准备好通过将内容提交到此自定义工作流来对其进行测试。

如果您有兴趣了解有关工作流及其高级功能的更多信息，请阅读 Application Builder 用户手册、Web Publisher 管理手册和 Workflow Manager 在线帮助。Documentum 还提供了一个名为 Business Process Manager（简称 BPM）的先进工作流管理工具，扩展了 Workflow Manager 提供的许多功能。讨论 BPM 及其功能超出了本书的范围。不过，您可能希望从 Documentum 的下载站点下载 BPM 软件并阅读其管理手册。

请转到下一章测试您在本章中创建的自定义工作流模板。

## 19.5 摘要

在本章中，我们讨论了工作流如何通过定义业务策略并制定内容在各种生命周期状态下移动的规范、条件以及参与用户和组，与生命周期协同工作。

我们为应用程序设计了一个非常简单的工作流，涉及内容作者、经理和审批者作为参与实体。

我们看到工作流模板是使用 Workflow Manager 工具创建的。每次使用工作流模板启动工作流时，Documentum 都会为其创建一个新的运行时实例。

工作流模板由通过流连接的活动组成。包被添加到流中，以便它们可以通过工作流路由内容。

我们简要讨论了工作流活动分为两种——手动和自动。手动活动由组或用户执行，而自动活动由系统执行。

最后，通过详细的步骤，我们了解了如何创建本章开头设计的自定义工作流模板。
