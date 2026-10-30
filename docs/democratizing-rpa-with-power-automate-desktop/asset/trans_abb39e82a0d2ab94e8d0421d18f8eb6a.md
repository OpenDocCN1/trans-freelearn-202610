

## 使用 PAD 进行分布式用户管理

让我们想象一下，我们负责为下个月入职我们小公司的用户设置账号。除了登录名和邮箱之外，当然还需要 SAP 登录名。人力资源部门已经在 Active Directory 中创建了账号，并在 Excel 电子表格中收集了这些用户的信息。我们的任务是在 SAP 中创建其中列出的用户，并通过电子邮件发送登录信息。这也包括创建初始密码。因此，我们需要一个流程来为我们执行以下操作：

- 从 Excel 电子表格中读取所有用户并提取数据
- 启动 SAPGUI 并调用创建新用户的相应事务（SU01）
- 遍历 Excel 数据，生成新密码，将其存储在表格的对应行中，并使用该数据在 SAP 中创建用户
- 向用户发送包含初始密码的电子邮件

### 存储 Excel 工作表并关闭 Excel 和 SAPGUI

由于我们没有用于自动化 SAPGUI 的单独操作，因此我们必须使用已知的技术远程控制该应用程序。然而，由于该应用程序也可以通过键盘输入出色地操作，我们希望尽可能避免创建 UI 元素和图像。使用键盘输入作为在应用程序中导航的选项有时会是一个优势，因为这样用户就不会依赖屏幕分辨率以及提前捕获所有 UI 元素的需求。

此外，这次我们希望应用我们已证实的将整体流程划分为更小子流程的做法。完整的流程将如下所示：

顾名思义，该区域启动 SAPGUI，该程序必须安装在您的计算机上。程序在第一个操作中直接调用。该程序由两个窗口组成——一个用于选择要登录的系统，另一个用于登录和运行程序。为了能够在后续流程中直接寻址这些窗口，这些窗口以 UI 元素的形式定义。为了做到这一点，最好先手动启动一次程序，并通过 PAD 将两个窗口定义为 UI 元素。

要启动 SAPGUI 程序，我们可以实现如下流程：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_226_0.png)

图 8.3 – 使用 PAD 启动 SAPGUI

首先，我们必须通过 **Run application** 操作（第 1 行）运行主 `saplogon.exe` 可执行文件。该可执行文件在 SAP 客户端软件的标准安装后可用。这基本上等同于在 Windows 桌面上双击相应的快捷方式。第 2 行中，我们等待窗口出现；之后我们将看到 **SAP Logon** 屏幕，在此我们可以选择要登录的系统：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_227_0.png)

图 8.4 — SAP 系统选择

正如我们所见，我只注册了一个系统。因此我只需在此处发送按键（第 3 行）按下 *Enter*。如果您注册了多个系统并希望登录到特定系统，则需要确保首先选中了正确的条目。

第 4 行中，我们等待主 SAP 窗口出现，其外观如下：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_228_0.png)

图 8.5 – SAP 登录屏幕

如前所述，该软件也设计为仅使用按键操作。我们可以通过 UI 元素的淡红色边角看到光标所在位置——在本例中为用户名文本框。同样，这是该软件的默认行为，我们可以利用这一点。然而，如果我们需要登录到不同的客户端（参见前一屏幕截图中的 Client 字段），我们就需要确保这些设置已完成。

在流程的第 5 行和第 6 行中，我们输入用户名和密码。要从一个字段跳转到下一个字段，我们只需使用 Tab 键。这些值存储在两个输入变量中，并标记为敏感。最后一步是按 Enter 并等待登录过程完成（第 6 行和第 7 行）。此时，我们已成功登录系统，可以开始在 SAP 中使用程序。

### “Launch_Excel” 和 “Launch_Outlook” 子流程

我们主流程中的下一步是查找需要在 SAP 系统中注册的用户，然后通过

由于我们希望将生成的密码存储在用户的对应行中，因此我们此时不会关闭 Excel。从这里开始，我们可以开始处理每个用户。

### 用户数据处理

现在我们已经启动了 SAPGUI 以及 Outlook 和 Excel，我们准备好处理每个用户的数据了。我们必须做的第一件事是在 SAP 中调用一个名为 SU01 的特定事务——这正是我们在主流程第 4 行中所做的。

### SAP 和事务代码

在使用 SAP ERP 时，我们经常谈论所谓的报表和事务，它们用于执行业务需求和流程。事务代码（例如 SU01）启动一个用于输入参数的程序，并且非常经常随后启动一个报表。此处使用的事务例如允许输入用户名，可以对其进行搜索或创建。因此，这里展示的过程代表了使用 SAP 的非常典型的例行操作。

以下屏幕截图显示了此步骤的操作参数对话框：

**Send keys**

Sends keys to the application that is currently active (More info)

**Select parameters**

| Parameter | Value |
| --- | --- |
| Send keys to | UI element |
| UI element | `appmask > Window 'SAP'` |
| Text to send | Input as text, variable or expression {x} |
|  | `su01{Return}` |
| Insert special keys | (button) |
| Insert modifier | (button) |
| Delay between keystrokes | 10 |
| Send Text as hardware keys | |
| On error | |
| Save | (button) |
| Cancel | (button) |

图 8.7 – 调用 SAP 事务 “SU01”

我们在这里更仔细地查看此步骤的原因是，我们将在本示例中始终使用这一基本概念：

- 因为我们也可以使用此操作选择之前定义的 UI 窗口，所以我们不必注意该窗口是否也在前台。这在流程中处理多个程序且无法始终知道当前哪个窗口在前台时尤其有用。通过指定 UI 元素，按键将被发送到指定的窗口——在我们的例子中是 SAPGUI。
- 我们可以通过键盘输入完全完成任务，使我们独立于按钮或列表项等额外的 UI 元素。因此，此流程也独立于屏幕分辨率。
- PAD 操作还允许我们执行特殊按键，例如 *Shift + Tab*（在界面中向后跳转一个字段）或 *Ctrl + S*（保存的快捷方式，也适用于 SAP）。
- 除此之外，我们可以以毫秒为单位指定按键之间的时间。这在 SAPGUI 在较慢的计算机上运行时很有用。

执行此操作后，我们将进入 SAP 中维护用户的屏幕。接下来，我们遍历循环（第 5 行至第 9 行），其中我们完成以下子任务：

- 确定要处理的用户信息（`Read_ExcelUserRow` 子流程）。我们之前读取了用户表。
- 使用该信息在 SAPGUI 中输入（`Enter_SAPUserData` 子流程）。
- 向用户发送包含初始登录凭据的电子邮件。

对于循环本身，我们并不是使用 `For each` 循环遍历行列表。相反，我们可以使用基于索引的方法，因为这使得更容易将数据写回 Excel 中的正确行。让我们看看 `Read_ExcelUserRow` 子流程的细节，看看这是如何工作的：

图 8.8 – 读取用户数据并生成随机密码

请注意，我们在第 1 行立即为登录创建随机密码。我们使用文本操作组中的 Create random text 操作。此操作正是为此目的而创建的，因为可以定义长度和复杂性等可能性。第 2 行至第 6 行中，我们创建流程变量，以便在流程后续更轻松地使用这些数据。第 7 行负责将生成的密码保存到 Excel 工作表的对应行中。在这里，我们使用了 LoopIndex 变量，其中包含我们当前正在处理的行的编号。

Subflows: Read_ExcelUserRow, Enter_SAPUserData, Launch_Outlook

| Step | Action | Description |
| --- | --- | --- |
| 1 | Create random text | Create a random text of length between 8 and 10 characters using uppercase letters, lowercase letters, symbols and digits and store it in InitialPassword |
| 2 | Set variable | Assign to variable UserName the value ExcelData[LoopIndex][0] |
| 3 | Set variable | Assign to variable UserFirstName the value ExcelData[LoopIndex][1] |
| 4 | Set variable | Assign to variable UserLastName the value ExcelData[LoopIndex][2] |
| 5 | Set variable | Assign to variable UserEmail the value ExcelData[LoopIndex][3] |
| 6 | Set variable | Assign to variable UserAlias the value ExcelData[LoopIndex][4] |
| 7 | Write to Excel worksheet | Write the value InitialPassword into cell in column 6 and row LoopIndex + 2 of the Excel instance ExcelInstance |

在下一个子流程中，我们再次使用 SAPGUI 并操作程序以创建新用户。如前所述，这完全可行

