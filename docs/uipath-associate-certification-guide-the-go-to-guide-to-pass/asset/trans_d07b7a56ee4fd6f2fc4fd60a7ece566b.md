

### 忽略

调试时，您可以使用 **Ignore** 操作跳过当前异常，并从流中的下一个活动恢复执行。

您可以通过忽略或跳过抛出异常的活动，继续调试其余活动。

> **注意**
> 当抛出异常且调试过程进入暂停状态时，**Retry** 和 **Ignore** 操作可用。

### 重新启动

在调试过程中，有时如果您想从头开始调试项目，请选择 **Restart** 操作。该操作将从项目的第一个活动重新启动调试过程。

### 中断

调试过程启动后，**Break** 操作即可用。您可以随时使用 **Break** 操作暂停调试过程。应用 **Break** 操作后，当前正在执行的活动将停止并在 Studio 中高亮显示。

根据情况，您可以选择 **Continue** 操作来恢复调试过程，或选择 **Step Into**、**Step Out** 或 **Stop** 操作。

### 聚焦

理想情况下，在调试过程中，当发生异常或遇到断点时，执行会进入暂停状态。**Focus** 操作可让您在调试流程处于暂停状态时，返回流中提到的断点或错误位置。**Focus** 操作是返回该特定活动并恢复调试过程的简便方法。

### 慢步

**Slow Step** 操作可在调试前或调试过程中启用。启用后，**Slow Step** 操作会高亮显示正在执行的活动。**Slow Step** 操作提供四种不同速度 – _1x_、_2x_、_3x_ 和 _4x_。

每次单击 **Slow Step** 按钮，调试速度会在 1x 到 4x 之间变化一级。因此，使用 **Slow Step** 以 1x 调试运行最慢，以 4x 最快。速度决定调试器从一个活动跳转到下一个活动的快慢。

### 执行轨迹

**Execution Trail** 显示调试过程中的执行路径。默认情况下，**Execution Trail** 按钮处于禁用状态。在执行每个活动时，它们会根据执行状态被高亮显示并用颜色代码标记：

- 部分执行的活动标记为金色黄色时钟符号：

![](img/84b06ad2832c53c5f677f8ce4c80f184_397_0.png)

- 成功执行的活动标记为绿色复选框符号：

![](img/84b06ad2832c53c5f677f8ce4c80f184_397_1.png)

- 抛出异常的活动标记为红色：

![](img/84b06ad2832c53c5f677f8ce4c80f184_397_2.png)

### 高亮显示元素

如果启用了 **Highlight Elements** 操作，则在调试过程中，每个活动的 UI 元素都会被高亮显示。因此，您可以将其与常规调试和逐步调试一起使用 – 例如，如果将 `Type Into` 活动放置在 Web 表单的文本字段上，在执行期间，Web 表单的文本字段会在机器人输入该文本字段之前先被高亮显示。

**Log Activities** 帮助您确定流中可能出现问题的位置。如果启用了 **Log Activities** 操作，所有调试的活动都会在 **Output** 面板中显示为 _跟踪日志_。如果禁用日志功能，则 _跟踪日志_ 不会在 **Output** 面板中显示；相反，您只能看到调试开始和结束时间以及任何抛出的异常。

此外，如果已连接到 UiPath Orchestrator，日志会自动发送到 Orchestrator。如果在 **Add** 或 **Edit** 用户窗口的 **Robot Settings** 选项卡中禁用 **Allow Development Logging** 选项，则可以将日志本地存储。

> 注意
> **Highlight Elements** 和 **Log Activities** 操作只能在调试前启用，并且在下次重新打开自动化项目时会保持启用状态。

### 发生异常时继续

在调试过程中启用 **Continue on Exception** 操作且抛出异常时，该异常会在 **Output** 面板中记录，并且执行会在不停止调试过程的情况下继续进行后续活动。

### 打开日志

单击 **Open Logs** 按钮将打开本地存储的日志文件夹。存储日志的文件夹路径是 `%localappdata%\UiPath\Logs`。日志的命名约定是 `YYYY-DD-MM_Component.log` – 例如，`2021-07-08_Studio.log`。

在本节中，您已了解功能区上的调试操作菜单。在本章的下一节中，您将看到 UiPath Studio 中可用的不同调试面板。

## 使用调试面板

除了 **DEBUG** 功能区中可用的操作外，UiPath Studio 还提供了一系列面板来帮助您进行调试过程。

UiPath 中有五个可用于调试的面板：

- 1. Locals 面板
- 2. Watch 面板
- 3. Immediate 面板
- 4. Call Stack 面板
- 5. Breakpoints 面板

以下截图中标记了每个面板。每个数字对应前面列表中面板的位置：

![](img/84b06ad2832c53c5f677f8ce4c80f184_399_0.png)

图 13.5 – 调试面板

一旦调试开始，所有五个面板都会变得可见。但是，首先，让我们来看看每个面板并了解它们的用途。

### Locals 面板

Locals 面板窗口有三个属性 – Name、Type 和 Value。该面板显示带有相应属性的以下信息：

- Variables：显示执行容器范围内的所有变量
- Arguments：显示执行工作流的所有参数
- Activity Properties：显示先前执行的活动和当前活动的属性
- Exceptions：显示异常的描述和类型

| Name | Type | Value |
|---|---|---|
| Variables | | |
| firstName | String | "Jane" |
| lastName | String | "Dorsy" |
| companyName | String | "MediCare" |
| row | DataRow | DataRow HasErrors=false... |
| DT_Contact | DataTable | [First Name,Last Name...] |
| ContextTarget | Object | Browser ... |
| Properties of Assign 'company name' (previous) | | |
| To | OutArgument | "MediCare" |
| Value | InArgument | "MediCare" |
| DisplayName | String | "Assign ' company name ' -" |
| Properties of Type into 'INPUT' (current) | | |
| Text | InArgument<string> | InArgument<string> ... |
| DeselectAfter | InArgument<Boolean> | ... |
| SimulateType | InArgument<Boolean> | ... |
| SendWindowMessages | InArgument<Boolean> | ... |
| AlterIfDisabled | InArgument<Boolean> | ... |
| DelayMS | InArgument<int32> | ... |
| DelayBefore | InArgument<int32> | ... |
| DelayBetweenKeys | InArgument<int32> | ... |

图 13.6 - Locals 面板

Locals 面板中的所有 Arguments、Properties 和 Variables 类别都可以压缩或展开。

当调试处于暂停状态时，单击 Value 属性下的铅笔图标按钮，可以修改变量和参数的值：

![](img/84b06ad2832c53c5f677f8ce4c80f184_400_0.png)

图 13.7 - 通过单击铅笔图标，将 firstName 变量的值从 Jane 更改为 Michael

同样，当调试处于暂停状态时，您可以单击放大镜按钮来检查 Value 属性列中项目的值：

![](img/84b06ad2832c53c5f677f8ce4c80f184_401_0.png)

图 13.8 – 通过单击放大镜按钮检查 DataTable 变量中的所有行

### Watch 面板

Watch 面板窗口有三个属性 – Expression、Type 和 Value。

您可以显示执行容器范围内的变量、参数和用户定义表达式的值。Watch 面板中的值会根据调试过程中相关活动的执行情况进行更新 – 例如，调试 For Each 活动内部的过程。Watch 面板中的值会在每次迭代时更新：

![](img/84b06ad2832c53c5f677f8ce4c80f184_401_1.png)

图 13.9 – Watch 面板显示变量和表达式的类型和值

