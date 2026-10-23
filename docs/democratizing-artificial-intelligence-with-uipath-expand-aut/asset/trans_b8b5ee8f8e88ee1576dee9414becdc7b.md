

6. 在 **Invoke Workflow File** 活动中，单击 **Import Arguments** 并将 `in_TargetFile` 参数设置为 `CurrentFile.FullName.` 这将确保 Main-Attended 工作流对文件夹内的每个文件运行：

![](img/05268897c8ac5d80eb3fead1cc8a822a_256_0.png)

图 8.43 – 已完成的 Dispatcher.xaml 工作流

通过添加一个带有 For Each File in Folder 活动的工作流，我们可以持续调用使用 Document Understanding Process 模板创建的 Main-Attended.xaml 工作流。在本节中，我们创建了一个新的工作流 Dispatcher，并将文件设为主项目的核心文件。在 Dispatcher.xaml 中，我们添加了一个文件夹选择器活动，以提示最终用户输入文件夹路径。然后我们添加了一个 for-each 循环来遍历文件夹中的文件，为每张收据调用 Main-Attended.xaml。在下一节中，我们将在部署自动化到生产环境之前添加一些额外的提示来指导最终用户完成自动化执行。

## 添加用户输入提示

最佳实践的 attended 自动化始终应包含提示或消息框，以指导和/或告知用户自动化的进度。如第 5 章《Designing Automation with End User Considerations》中所述，让自动化易于使用是确保自动化采用的简单方法。在本节中，我们将着手添加消息框和信息对话框，以在自动化执行期间让最终用户保持了解。

在上一节创建调度器时，我们添加了一个文件夹选择器来提示人工用户选择收据文件夹以供自动化提取。新的用户可能会发现此文件夹选择器令人困惑，因为他们可能不知道该选择什么，因此我们应在文件夹选择器之前添加一个消息框，指导用户选择哪个文件夹：

- 1. 在 Dispatcher.xaml 中，在 Browse for folder 活动之前拖入一个 Message Box 活动（图 8.44）。
- 2. 在 Text 输入属性中添加一个描述性消息，例如 Please choose a receipts folder：

![](img/05268897c8ac5d80eb3fead1cc8a822a_257_0.png)

图 8.44 – 向 Dispatcher 添加消息框

通过在呈现文件夹选择器之前添加消息框，我们可以更准确地指导用户选择自动化要交互的正确文件夹。虽然添加消息框是让用户保持了解的一种方法，但另一种方法是呈现状态栏，让用户了解自动化的状态（图 8.45）：

![](img/05268897c8ac5d80eb3fead1cc8a822a_258_0.png)

图 8.45 – 呈现状态栏

UiPath 团队创建了一组自定义活动，专门用于通过状态栏、进度条和消息框让用户保持更新。使用 Interactive Activities 自定义活动，我们可以快速轻松地将这些消息添加到工作流中。

> Important Note
> 有关 Interactive Activities 的更多信息，请访问 https://marketplace.uipath.com/listings/notification-activities。

要在项目中安装 Interactive Activities，请按照以下步骤操作：

- 1. 在 Note Receipt Details 项目的 Design 功能区中单击 Manage Packages。
- 2. 在 Manage Packages 窗口中，单击 Marketplace 并搜索 UiPathTeam.Interactive.Activities（图 8.46）：

![](img/05268897c8ac5d80eb3fead1cc8a822a_258_1.png)

图 8.46 – 安装 Interactive Activities

- 3. 要安装，请选择最新版本并单击 **Save**。

安装 Interactive Activities 后，即可将其添加到项目中。

为了最好地显示用户的进度，我们应在开始和完成收据时呈现状态栏，以及在 Document Understanding 框架的里程碑过程中呈现状态栏（数字化、分类、提取和导出）。让我们先在调度器中添加一个开始消息。

- 4. 在 **For Each File in Folder** 活动的 **Do** 序列内拖入一个 **Display Message** 活动。
- 5. 在 **Display Message** 活动中，在 **Message** 输入属性中添加一个描述性消息，并在 **Handle** 输出属性中创建 `windowIdentifier` 变量。

如果您想多次替换状态栏消息，**Handle** 输出属性非常重要。句柄充当状态栏窗口的唯一标识符，如果在更新状态栏时未设置该属性，将导致多个状态栏彼此堆叠（*Figure 8.47*）：

![](img/05268897c8ac5d80eb3fead1cc8a822a_259_0.png)

图 8.47 – 状态栏彼此堆叠

- 6. 在 `Variables` 窗格中，将 `windowIdentifier` 的作用域设置为 main sequence 而不是 **Do**（*Figure 8.48*）。这可确保在 for 循环的每次迭代中唯一标识符不会被重置：

| Name | Variable type | Scope |
|---|---|---|
| selected_folder | String | Main |
| windowIdentifier | Int32 | Main |
| Create Variable | | Do |
| | | Main |
| | | Main |

图 8.48 – 设置 windowIdentifier 的作用域

添加 **Display Message** 活动后，您的调度器工作流应如下图所示 *Figure 8.49*：

![](img/05268897c8ac5d80eb3fead1cc8a822a_260_0.png)

图 8.49 – 带有显示消息的调度器工作流

可以在 Document Understanding Process 模板内部添加额外的状态消息，以调用数字化、分类、提取和导出。您可以将 **Display Message** 活动添加到每个单独的工作流中；只需确保传递在调度器工作流中设置的 `windowIdentifier` 以防止状态彼此堆叠。

通过添加消息框和状态消息，我们使自动化对最终用户更友好一些，通过指导他们选择哪个文件夹并让他们了解自动化的状态。现在这些活动已添加到工作流中，我们可以认为自动化已完成，并进入下一节部署自动化到生产环境。

## 部署到生产环境

在生产环境中，我们不应直接从 UiPath Studio 运行自动化，也不应期望最终用户能够访问 UiPath Studio。因此，我们必须将自动化部署到 UiPath Orchestrator，以便其他人可以从他们的 UiPath Assistant 运行我们的自动化。

UiPath Assistant 是一款桌面工具，用于启动和与 UiPath 自动化交互，是一种启动板（图 8.50）：

![](img/05268897c8ac5d80eb3fead1cc8a822a_261_0.png)

当自动化部署用于大规模使用时，它会部署到 UiPath Orchestrator，然后它会出现在最终用户的助手上供他们运行。

> **Important Note**
> 有关部署自动化的更多信息，请访问 https://docs.uipath.com/studio/docs/about-publishing-automation-projects。

对于此用例，我们将快速将自动化发布到我们自己的 UiPath Assistant 中的个人工作区：

- 1. 在 **Note Receipt Details** 项目中，单击 UiPath Studio 的 **Design** 功能区中的 **Publish** 按钮。
- 2. 检查 **Publish to** 字段是否设置为 **Orchestrator Personal Workspace Feed**（*Figure 8.51*）：

![](img/05268897c8ac5d80eb3fead1cc8a822a_262_0.png)

图 8.51 – 发布自动化

- 3. 单击 **Publish**。

发布后，您应该能在您的 UiPath Assistant 中看到自动化（图 8.52）：

![](img/05268897c8ac5d80eb3fead1cc8a822a_263_0.png)

图 8.52 – 项目部署到 UiPath Assistant

单击 Note.Receipt.Details 右侧的 Run 按钮将把自动化安装到您的机器上并开始执行。

部署后，您可以自由地直接从 UiPath Assistant 运行自动化。对代码的任何更改都可以在 UiPath Studio 中进行，然后按照刚才讨论的步骤重新发布。在本节中，我们通过将 Note Receipt Details 项目部署到我们的 UiPath Assistant 来完成了用例构建——干得好！

