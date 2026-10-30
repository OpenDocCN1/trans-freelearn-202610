

也提供了一个参数来定义是仅关机、重启、休眠，还是将工作站设为睡眠。

+   - **Print: Print document/Get default printer/Set default printer:** 这些操作使用文档名称的文本字段，获取或设置打印机。
+   - **Screen: Get screen resolution/Set screen resolution/Control screen saver:** 这些操作需要一个已连接的控制台会话才能工作。此会话在用户登录到工作站并且 PAD 由该用户执行时可用。这意味着用户已登录并启动了 PAD 控制台。在无人值守场景中，这些操作没有任何效果。设置屏幕分辨率有助于以标准化方式运行场景，例如使用屏幕坐标的场景，如鼠标移动（参见下一节）。
+   - **Miscellaneous other actions: Show desktop/Play sound/Empty recycle bin/Take screenshot:** 这里值得一提的是，**Show desktop** 既可以显示桌面，也可以恢复所有窗口。此外，**Take screenshot** 对于收集和记录有关系统的信息非常有用，适用于整个桌面或前台窗口。

在查看另一个示例之前，让我们先来看一下键盘和鼠标的操作。

## Mouse, keyboard, and clipboard

该组中的操作完成了本章关于模拟用户会话的可能性，并提供了真实用户可以执行的所有操作，从特定的鼠标序列到发送快捷键和预定义键组合。以下是详细信息：

+   - **Block Input**: 此操作有一个开关，可阻止或解除阻止用户进行的任何按键或鼠标点击。这在执行期间很有用，可以防止用户额外的按键或鼠标点击干扰流程。但请注意。一方面，请务必在流程的末尾插入 `Unblock` 操作。并且
请不要在中间创建断点，因为这会停止流程，并且由于输入被阻止，没有机会继续流程。
+   - **Get mouse position**: 这会将鼠标指针的坐标存储在两个变量中（x 位置和 y 位置），相对于屏幕或前台窗口。
+   - **Move mouse / to image / to text on screen (OCR)**: 可用于将鼠标指针移动到屏幕上的特定位置。你也可以将鼠标指针放置在所需位置并按 Ctrl + Shift。这会将当前坐标传输到操作参数中。还有多种运动行为，从 **instant** 到 **with animation (high speed)**。使用 **Move mouse to image** 操作时，可以定义或选择一个图像（参见 *第 5 章* 关于图像）来移动鼠标，甚至在移动后发送点击。PAD 还可以通过使用 **光学字符识别 (OCR)** 技术来检测屏幕上的文本。**Move mouse to text on screen (OCR)** 允许你选择 OCR 引擎、应查找的文本以及一些附加选项。例如，如果你已确定屏幕上想要执行鼠标点击的区域，该区域可能带有文本标签，则可以使用此操作成功地将鼠标移动到该区域并触发点击。
+   - **Send mouse click**: 此操作可以发送各种鼠标事件，从标准的左键单击和右键单击、双击到中键单击。也可以仅发送按钮按下事件，并在另一个时间发送按钮抬起事件。此操作还包括一个开关，允许将鼠标指针移动到特定坐标，这本质上与 **Move mouse** 操作相同。
+   - **Wait for mouse**: 当应用程序接收到鼠标点击时，鼠标光标可能会变为 **wait** 光标或 **hourglass** 光标。此操作可用于等待光标再次变为普通光标/不变为普通光标。

通过这些鼠标控制操作，可以模拟工作站前可能不存在的硬件，例如，如果鼠标上没有中键。我们还可以实现并模拟 *click-and-drag*
或 *drag-and-drop* 操作。然而，大多数应用程序不仅可以通过鼠标操作，通常还可以通过键盘直接输入的一整套快捷键立即触发操作。为了使 PAD 也能使用这种非常高效的操作方式，提供了以下键盘操作：

**Send keys**: 如果我们想在前台窗口/窗口实例/标题或类或 UI 元素中光标当前位置输入一些文本，这就是要使用的操作。以下屏幕截图显示了此操作的<|message|>

## Send keys

向当前活动的应用程序发送按键 更多信息

## Select parameters

Send keys to: Foreground window

Text to send: Input as text, variable or expression

Insert special keys Insert modifier

Delay between keystrokes: 10

Send Text as hardware keys:

On error Save Cancel

以下示例使用了我们刚刚学习的一些操作。我们想要创建一个流程，收集有关当前系统的一些信息，将其存储在 PowerPoint 幻灯片中，并将该报告保存到 OneDrive。这将需要一系列鼠标点击和按键发送来完成此操作。

请注意，我们在此示例中没有使用 UI 元素，这也将是一个非常好的选择。相反，我们想要使用之前定义的操作来模拟用户会话。所以，让我们看看流程是什么样的：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_211_0.png)

Figure 7.15 – A flow to create a PowerPoint system report

正如我们在这里看到的，流程再次被结构化为一些子流程，以便更易于阅读和维护。第一个子流程通过使用 Windows 按钮打开 PowerPoint，第二个子流程使用当前日期创建第一个介绍幻灯片。第三个子流程启动 Windows 系统信息窗格，截取屏幕截图并将其放入剪贴板。在第四个流程中，此屏幕截图被插入到第二张幻灯片中，经过一些格式设置后，我们将幻灯片保存并关闭到
默认的 OneDrive 文件夹。所有子流程都嵌入在 **Block Input** 操作中，其中第一个打开阻止，第二个解除阻止。

让我们看看第一个子流程的细节：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_212_0.png)

Figure 7.16 – Launch PowerPoint with the Send mouse click action

在第一个操作中，我们触发对 Windows 按钮的点击以激活 **Start** 菜单。这里显示的坐标是我的桌面的坐标。在另一个桌面上可能不同。

Different desktops for flow creation and execution

我们稍后会看到，可以使用一台计算机进行流程创建，而潜在的另一台（或多台）计算机用于流程执行。当 **Send mouse click** 操作在 UI 流程中使用时，建议确保用于创建的计算机与用于执行的计算机具有相同的屏幕分辨率（标准化环境）。否则，**Send mouse click** 操作的坐标
可能无法工作。或者，我们也可以使用屏幕分辨率的操作来为执行桌面建立标准化

在第二个操作中，我们只需使用键盘输入将 Powerpoint 文本发送到开始菜单，这会触发对相应程序的搜索。在最后一步中，我们再次使用鼠标点击来启动搜索列表中的第一个程序。这里也适用关于坐标的注释。这将启动 PowerPoint，我们可以继续下一个子流程：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_213_0.png)

Figure 7.17 – Creating the first slide with PAD

在第一个操作中，我们点击 PowerPoint 中标准布局的图像。**Move mouse to image** 操作允许我定义一个我刚刚在该操作中创建的图像。这现在会导致 PowerPoint 显示具有默认布局和第一个起始幻灯片的演示文稿。

接下来的两个步骤确定当前日期和工作站的名称。对于后者，使用了 PowerShell 的脚本操作。在此子流程的最后一步中，由 **System Report** 文本和当前日期组成的标题被插入到幻灯片中。我们稍后在保存文件时会使用工作站名称。这就把我们带到了下一个子流程：

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_214_0.png)

Figure 7.18 – Create a screenshot for System information with PAD

此子流程再次使用 **Send mouse click** 操作来访问 Windows **Start** 菜单，但这次我们使用右键单击事件来启动

