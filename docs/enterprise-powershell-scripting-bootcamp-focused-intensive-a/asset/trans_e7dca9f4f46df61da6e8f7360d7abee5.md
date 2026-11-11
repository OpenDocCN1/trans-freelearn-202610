
## 摘要

本章中，您了解了 Windows 服务以及如何在 Windows 服务中停止、启动和设置项目。您继续学习了 Windows 进程以及如何获取、停止和启动进程。您学习了如何通过系统上运行的进程扫描系统以查找已登录用户。您通过使用 PowerShell 学习如何评估系统上的用户配置文件来结束本章。

在下一章中，您将学习如何与系统上的计划任务进行交互。

## 6
## 评估计划任务

企业利用计划任务实现多种目的。无论是处理业务交易还是提供健康检查，它们已成为按计划运行操作的标准。计划任务具有灵活性，因为它们也可以在登录时、在触发特定事件后甚至手动运行。计划任务可以运行可执行文件，甚至可以启动脚本以扩展自动化无限的可能性。

由于计划任务用途广泛，使用 Windows 服务器扫描脚本扫描它们非常重要。这将帮助您识别仅在触发或按计划运行的进程，例如季度报告或夜间进程。它还可以用于识别用于运行这些计划任务的备用用户帐户。由于计划任务非常灵活，因此必须捕获计划任务正在执行的操作、如何调用它们以及指定运行任务的帐户。

本章中，您将：

- 了解如何在系统上查看计划任务
- 创建计划任务触发器、操作和设置集对象
- 利用触发器、操作和设置集创建计划任务对象
- 在系统上注册和注销计划任务
- 使用新操作更新现有计划任务
- 识别作为备用用户帐户运行的计划任务

### 计划任务

计划任务使您能够在系统上安排自动化操作。Microsoft 具有各种内置计划任务，其中包括时间同步、通用即插即用、Windows 更新和磁盘碎片整理。这些任务操作由不同的触发条件调用并自动在系统上运行。

您可以使用 get-scheduledtask cmdlet 检索系统上的计划任务列表。get-scheduledtask cmdlet 显示计划任务名称、其 taskpath 和当前状态的列表。您还可以返回其他属性，例如作者、日期、说明、版本和文档。

要检索和计算系统上的计划任务，您可以利用以下内容：

```
get-scheduledtask
(get-scheduledtask).count
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_99_0.png)

此示例显示如何检索系统上的计划任务以及如何计算任务总数：

1. 您首先通过调用 get-scheduledtask cmdlet 开始。您将在系统上看到大量计划任务列表。
2. 然后您使用 .count 方法执行 get-scheduledtask cmdlet。此示例中的系统返回 165 个计划任务。

创建计划任务所需的两个组件是任务操作和任务触发器。任务触发器启动计划任务。最常见的触发器是按计划、在登录时、在启动时、在空闲时、在事件上和手动。在使用触发器时，您必须利用 New-ScheduledTaskTrigger cmdlet 创建新的任务触发器对象。然后您需要将任务触发器对象存储在变量中以创建计划任务。

要按计划创建计划任务触发器，您可以执行以下操作：

```
$schTrigger = New-ScheduledTaskTrigger -Daily -DaysInterval 1 -At "23:00"
$schTrigger
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_100_0.png)

在此示例中，您学习如何创建计划任务触发器：

1. 您首先通过调用 New-ScheduledTaskTrigger cmdlet 开始，使用 -Daily 参数、使用 1 属性的 -DaysInterval 参数以及设置为 23:00 属性的 -At 参数。
2. 然后您将此计划任务触发器对象存储在 $schTrigger 变量中。当您调用 $schTrigger 变量时，它返回触发器 Id 为 0，频率为 Daily，时间为 2/18/2017 11:00:00 PM，DaysOfWeek 为 $null，并且 Enabled 设置为 true。

> 有关计划任务触发器及其语法的更多信息，您可以浏览至： https://technet.microsoft.com/itpro/powershell/windows/scheduled-tasks/new-scheduledtasktrigger.

任务操作定义计划任务正在执行的内容。任务操作可以简单到运行可执行文件，也可以调用脚本执行各种操作。您使用 New-ScheduledTaskAction cmdlet 创建新的操作对象。除了 cmdlet 之外，您还可以利用 -Execute 参数启动可执行文件。您还具有 -Argument 参数，它使您可以向正在调用的可执行文件传递参数。您需要将任务操作对象存储在变量中以创建计划任务。

要创建打开 calc.exe 的计划任务操作，您可以执行以下操作：

```powershell
$schAction = New-ScheduledTaskAction -Execute "Calc.exe"
$schAction
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_101_0.png)

在此示例中，您学习如何创建计划任务操作：

1. 您首先通过调用 New-ScheduledTaskAction cmdlet 开始，将 -Execute 参数设置为 calc.exe 属性。
2. 然后您将此计划任务对象保存在 $schAction 变量中。

当您调用 $schAction 变量时，它返回 Id 为 $null，Arguments 为 $null，Execute 为 Calc.exe，WorkingDirectory 为 $null 和 PSComputerName 为 $null。如果向 new-ScheduledTaskAction cmdlet 传递了其他参数，$null 字段将填充数据。

配置的可选组件是任务设置集，它为计划任务本身提供配置。如果您想要包含其他参数，可以配置电池上的执行、执行持续时间、空闲持续时间、重启计数和重启间隔。您可以使用 New-ScheduledTaskSettingsSet cmdlet 定义计划任务设置集，并且您需要将其存储在变量中以创建计划任务。

要使用 -DisallowDemandStart、-Hidden 和 -DisallowHardTerminate 参数创建计划任务设置集，您可以执行以下操作：

```
$schSettingSet = New-ScheduledTaskSettingsSet -DisallowDemandStart
-Hidden -DisallowHardTerminate
$schSettingSet
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_102_0.png)

在此示例中，您创建一个新的计划任务设置集：

1. 您首先通过声明 New-ScheduledTaskSettingsSet cmdlet 开始，使用 -DisallowDemandStart、-Hidden 和 -DisallowHardTerminate 参数。
2. 然后您将计划任务设置集对象设置为 $schSettingSet 变量。

当您调用 $schSettingSet cmdlet 时，您会显示计划任务设置集具有的所有参数。在此设置集中，您可以看到 AllowDemandStart 设置为 False，AllowHardTerminate 设置为 False，Hidden 设置为 True。

> 有关计划任务设置及其语法的更多信息，您可以浏览至： https://technet.microsoft.com/itpro/powershell/windows/scheduled-tasks/new-scheduledtasksettingsset.

在将任务操作和任务触发器捕获到变量后，您创建计划任务对象。您调用 New-ScheduledTask cmdlet，使用 -Action 参数引用任务操作对象，并使用 -Trigger 参数引用任务触发器对象。您还可以使用 -Settings 参数使用可选的任务设置集，通过引用任务设置集对象。您将所有这些项目放入将包含计划任务对象的变量中。

要创建计划任务变量，您可以执行以下操作：

```
$schAction = New-ScheduledTaskAction -Execute "Calc.exe"
$schTrigger = New-ScheduledTaskTrigger -Daily -DaysInterval 1 -At "23:00"
$schSettingSet = New-ScheduledTaskSettingsSet -DisallowDemandStart -Hidden -DisallowHardTerminate
$schTask = New-ScheduledTask -Action $schAction -Trigger $schTrigger -Settings $schSettingSet
$schTask
$schTask.Triggers
```

此命令的输出如下所示：

![](img/a27a138d6586c10902d8e86ab3384834_103_0.png)

此示例显示如何采用计划任务操作、触发器和设置集来创建计划任务对象：

1. 您首先通过调用 New-ScheduledTaskAction cmdlet 开始，将 -Execute 参数设置为 calc.exe 属性。
2. 然后您将此计划任务对象保存在 $schAction 变量中。
3. 然后您使用 New-ScheduledTaskAction cmdlet 将 -Execute 参数设置为 calc.exe 属性。
