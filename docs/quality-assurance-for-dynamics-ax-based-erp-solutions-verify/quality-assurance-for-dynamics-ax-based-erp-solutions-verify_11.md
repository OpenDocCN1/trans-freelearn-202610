## 代码分析器

代码分析器是在运行时跟踪代码执行历史的工具。代码分析器可用于以下用途：

- 测量每行、方法、对象等的执行时间
- 单元测试期间的代码覆盖率分析
- 识别性能瓶颈

代码分析器不仅可以通过图形用户界面调用，还可以通过 X++ 代码调用。我们将不讨论通过 X++ 调用它。使用代码分析器包括两个阶段：

- 收集数据
- 查看/分析数据

### 收集数据

为了收集执行数据，需要调用代码分析器，然后用户应遍历每个正在审查的功能/功能。应遵循以下步骤：

1. 通过转到 **工具 | 开发工具 | 代码分析器** 打开代码分析器工具。
2. 通过选中 **Trace Depth Enabled** 复选框并选择层级数来限制检查深度。
3. 在应用程序中导航至测试应开始的位置。
4. 在代码分析器窗口中单击 **Start**。
5. 在不同条件下使用功能，以确保每行代码至少执行一次。
6. 当所有待测试功能都已使用后，在代码分析器窗口中单击 **Stop** 按钮。
7. 在 **Summary** 对话框中编写代码分析的描述（供参考）。
8. 单击 **OK** 按钮，并在打开“Profiler runs”表单以查看数据时开始查看/分析数据的第二阶段。

> 不要选中 **Calculate line total** 复选框。

### 查看/分析数据

特定运行的数据视图可能具有不同的目标，例如性能分析、代码覆盖率分析等。代码分析器有四种不同的表单以不同格式查看数据。

每个表单都以 Ticks 显示时间计数。要将这些数据计算为毫秒或秒等任何单位，请将数据乘以“Profiler run”表单上提供的“Ticks/second”。

### 调用树表单

在此表单中，行按执行的相同顺序显示，第一个执行的行在顶部。这有助于理解表单中的流程和各种触发器。在此表单中，系统方法只有在从应用程序调用时才会出现。

### 配置文件行表单

此表单根据执行时间显示“All executed”行，执行时间最长的行在顶部。此表单也可用于查看给定测试用例集的代码覆盖率。

### 遍历表单

在此表单中，每个方法按执行时间顺序显示，执行时间最长的显示在最前面（尽管顺序可以更改）。除了持续时间外，它还显示对象被调用的次数以及总执行时间的百分比持续时间。此信息可用于优化性能优化的努力，或从测试角度来看，可用于定义性能问题的优先级。此表单分为三个部分。上部窗格显示

上部显示方法，下部（再次分为两个部分）显示所选方法的子调用和父调用。要选择一个方法，双击它，它将在下部显示子调用和父调用。

用于限制显示的调用类型，可以使用以下按钮：

| Button | Type of calls |
|---|---|
| Profile line | Opens the profile lines form to show all lines of code that are executed. |
| Setup | This has four items to limit the type of calls displayed
None: No filtering, view all fields.
Code hot spots: View only the Duration, Share – duration, Method calls, and Share – method calls fields.
Database access: View only the Method calls, Share – method calls, and Calculated SQL time fields.
Remote connection: View only the Method calls, Share – method calls, and Calculated AOS time fields. |

## Totals Forms

**Totals** 表单用于显示汇总数据。各种 **Totals** 表单可从 **Tools | Development tools | Code profiler | Profiler runs | Totals** 访问。

Line total form: Displays aggregated data about total amount of time spent on executing each line of code for a code profiler run.

Method total form: Displays aggregated data about total amount of time spent on executing each method for a code profiler run.

Object total form: Displays aggregated data about total amount of time spent on executing each object for a code profiler run.

以下是三个表单上可用按钮的摘要：

| Button name and Function | Line total form | Method total form | Object total form |
|---|---|---|---|
| Profile lines: Opens Profile lines form | Yes | Yes | No |
| Line total: Opens Line total form | No | Yes | Yes |
| Method total: Opens Method total form | No | No | Yes |

## Profile Summary Form

此表单用于计算代码分析器运行中每行代码、每个方法和每个对象的总执行时间。当在创建运行时在代码分析器表单上清除“计算行总计”复选框时使用此表单；否则总计计算会自动完成。

> 代码分析器工具将很快被 "Trace Parser" 跟踪分析器取代。
