### 理解人机界面（HMIs）

前三章涵盖了大量关于**可编程逻辑控制器（PLCs）**的内容，PLC 是大多数自动化机器的大脑。我们学习了如何使用 PLC、传感器和执行器来自动化工业过程。然而，在大多数情况下，仅靠 PLC、传感器和执行器无法为真实的工业过程提供所需的功能。**人机界面（HMI）**通常集成到生产线和其他工业过程中，以便用户轻松控制机器并获得机器状态的反馈。

在上一章中，我们学习了如何使用 PLC 控制机器，其中用户或操作员按下按钮启动机器，指示灯显示机器是否正在运行。将 HMI 集成到此类系统中会使其更易于使用。HMI 允许您启动或停止机器，甚至通过图形界面或触摸屏获得机器的反馈或状态更新。

在本章中，您将学习有关 HMI 的知识以及如何将其与 PLC 接口以向机器发出命令并从机器获取反馈。您将使用 TIA Portal 创建用于监控和控制的 HMI 屏幕。您将学习如何模拟您的 HMI 程序（在没有真实 HMI 面板的情况下测试 HMI 程序的好方法），您还将学习如何将设计的 HMI 图形下载到真实的 HMI 面板（西门子 HMI - KTP400）以用于实际应用。

我们将在本章中涵盖以下主题：

- 介绍 HMI
- 探索 HMI 的应用
- 理解 HMI 编程和开发
- 理解 HMI 编程软件
- 探索 HMI 制造商及其编程软件
- PLC 与 HMI 的接口
- 将程序下载到 PLC 和 HMI

#### 技术要求

您需要了解前三章中的 PLC、传感器和执行器。

确保您已在 PC 或笔记本电脑上下载并安装了 TIA Portal V13。请参阅第 8 章《使用 TIA Portal 理解 PLC 软件和编程》。

可能需要的其他设备包括西门子 HMI（KTP400）和西门子 PLC（S7 1200，CPU 1211C，AC/DC/RLY）。

#### 介绍 HMI

HMI 可以被视为允许人类和机器交互的图形界面。我们了解到 PLC 可以控制和自动化工业机器。另一方面，HMI 允许人类与机器交互或通信。不管我们如何自动化一个过程，我们仍然需要一个人类（操作员）来启动或发起该过程，执行其他功能，并获得有关机器运行情况的反馈。

HMI 允许人类向 PLC 发出命令以控制过程，并从 PLC 接收有关过程的反馈。有了 HMI，人类操作员可以通过 PLC 与机器交互、监控和控制机器。HMI 通常通过通信电缆连接到 PLC，具体取决于所使用的协议（Ethernet/IP、Profinet、Profibus 或 Modbus）。本章不讨论协议。我们将在第 13 章《工业网络和通信协议基础》中研究协议。然而，我们将使用 Profinet 作为建立 HMI 与 PLC 之间通信的协议。基本上，PLC 通过电缆连接到机器或传感器和执行器，正如我们在 *第 7 章*、*理解可编程逻辑控制器 – 第 1 部分* 中所学到的，我们在其中讨论了 PLC 布线。以下图表描绘了人类操作员、HMI、PLC 和机器，以及它们之间的通信流程：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_366_0.png)

图 10.1 – 人类操作员、HMI、PLC 和机器

HMI 并非仅限于制造和其他工业环境。还有许多我们拥有并经常使用却不知道它们是 HMI 的 HMI。例如，一些汽车具有触摸屏界面，驾驶员或乘客可以通过该界面控制空调、加热、音响和其他系统。这些触摸屏就是 HMI。驾驶员或乘客能够通过触摸屏启用的界面与机器交互。HMI 只是一个界面（通常是图形化的），人类可以通过它控制机器。它也可以被称为**人机界面（MMI）**、**操作员界面终端（OIT）**或**用户界面（UI）**。HMI 的设计目的是确保操作员可以轻松监控或控制机器。HMI 还设计用于确保操作员可以监控报警并相应地做出响应。各种颜色可用于表示报警的优先级级别。HMI 报警提供有关重要事件和故障的信息。HMI 的另一个优点是趋势。趋势可以指一段时间内数据的图形表示。HMI 允许您在图形界面中监控或查看一段时间内来自传感器的數據。

HMI 还支持设定点管理。HMI 可以设计和编程以允许操作员从图形界面轻松更改设定点值。

HMI 包括硬件和软件，允许人类操作员与机器之间进行通信。

下图显示了操作员（用户）可以使用的一种 HMI：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_367_0.png)

图 10.2 – HMI 显示 GUI，用户可以通过该界面与机器或过程交互

此图像的版权归 Wikimedia Commons 所有：
https://creativecommons.org/licenses/by-sa/4.0/deed.en

HMI 用图形界面取代了连接到 PLC 的开关和其他形式的控制和状态指示器，通常是触摸屏，如前图所示。HMI 允许人类操作员监控过程，启动或停止过程，调整设定点，并执行需要人类注意的其他控制功能。

#### 探索 HMI 的应用

HMI 用于几乎所有行业。一些工业应用领域包括以下内容：

- **食品和饮料**：HMI 可用于轻松监控和控制生产过程，以确保食品和饮料行业的质量。HMI 允许在生产中使用配方。“配方”简单地指用户可以根据要生产的产品类型选择的数据或值。配方总结了特定产品的生产数据或机器配置。然后在给定时间将数据从 HMI 传输到 PLC 或控制器以切换到另一种产品类型。当要生产多种产品时使用配方。配方不仅在食品行业有优势；它们在生产多种产品的其他行业也非常有益。
- **石油和天然气**：HMI 为钻井和钻机操作员提供了一种实时控制和监控现场应用的手段。HMI 还有助于石油和天然气的其他领域，例如炼油和管道完整性管理。
- **电力**：HMI 可用于变电站和电力系统的其他区域，以便轻松执行开关和其他控制操作。
- **废水管理系统**：HMI 用于废水处理，以查看处理系统的各个方面，包括 pH 值、化学品使用量和水箱液位。
- **交通运输**：通过人类与机器之间的交互，可以提高公共交通系统的性能、效率和安全性，而 HMI 使其成为可能。

我们刚刚了解了 HMI 及其在各个行业中的应用。我们将在下一节学习 HMI 编程和开发。

#### 理解 HMI 编程和开发

HMI 编程涉及机器操作、控制命令的视觉表示（图形）的创建，以及编写代码以执行所需功能的编写。

HMI 的主要目的是允许操作员控制机器或过程。因此，HMI 设计必须做到以下几点：

- 使用者能够轻松访问必要的信息和控制。
- 允许用户尽可能快速地导航到不同区域。
- 允许用户或操作员查看机器的当前状态。操作员必须能够通过 HMI 上的视觉指示判断机器是否正在运行或已停止。还可以提供其他信息来帮助操作员做出正确决策。

让我们继续在下一节学习有关 HMI 编程软件的知识。

#### 理解 HMI 编程软件

HMI 编程软件是用于设计图形界面并在必要时编写一些代码以实现用户（人类）与机器之间交互的专用软件。该软件通常在编程前安装在 PC 上，然后再进行



通过通信电缆下载到HMI面板，就像我们对PLC所做的那样。

不同品牌的HMI面板有不同种类的编程软件。下一节将讨论各种HMI制造商及其编程软件。

#### 探索HMI制造商及其编程软件

这里列出一些流行的HMI制造商：

- **西门子：** 西门子是一家德国公司，是自动化行业顶尖公司之一，生产SIMATIC HMI。其部分HMI型号包括KTP400、KTP600和KTP700。其HMI编程软件是WinCC。
- **罗克韦尔自动化：** 罗克韦尔自动化是北美顶级公司之一。其HMI品牌名为**PanelView**，包括PanelView 600、PanelView 800和PanelView+400。其HMI编程软件包括RSView32和FactoryTalk View。
- **施耐德电气：** 施耐德电气是欧洲顶级公司之一。其HMI品牌包括Magelis和Harmony。其HMI编程和配置软件包括Ecostructure Operator Terminal Expert和Vijeo Designer。
- **三菱电机公司：** 三菱电机公司（MELCO）是三菱的一个分部。其HMI产品线名为**图形操作终端（GOT）**，例如GOT 1000系列和GOT 2000系列。其HMI编程软件是GTworks2（GOT HMI设计器软件）。

我们刚刚了解了一些主要的HMI制造商及其软件。本书将重点介绍西门子HMI（KTP 400）及其软件（WinCC）。我们将在后续章节中学习如何将西门子HMI（KTP400）连接到PLC，以及如何在TIA Portal中使用WinCC开发HMI画面。

#### PLC与HMI的接口连接

在*第9章“深入探讨使用TIA Portal进行PLC编程”*中，我们学习了如何使用按钮来启动和停止，以及如何使用灯作为指示器来显示负载是开启还是关闭。

正如我们之前讨论的，HMI允许操作员通过PLC与机器交互。这意味着需要PLC与机器进行通信。

在本节中，我们将学习如何使用Profinet作为通信协议来连接西门子HMI（KTP 400）和西门子PLC。我们将使用TIA Portal对PLC进行编程，并使用WinCC开发HMI应用程序，WinCC是在*第8章“理解PLC软件并与TIA Portal编程”*中安装TIA Portal时安装的。

到本节结束时，您应该能够连接PLC和HMI，使HMI能够用于启动和停止，并获取机器（例如电机）状态的反馈。

我们已经熟悉西门子S7 1200 PLC（CPU 1211C）。下面的照片展示了一台西门子HMI（KTP 400）：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_372_0.png)

图10.3 – 西门子HMI面板（KTP 400）。（此图片致谢Showlight Technologies LTD. www.showlight.com.ng）

让我们开始吧：

- 1. 启动TIA Portal，单击“创建新项目”，输入项目名称（例如PLC-HMI-PRACTICE），如下面的截图所示。单击“创建”：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_372_1.png)

图10.4 – 创建新项目

##### 2. 单击“配置设备”：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_373_0.png)

图10.5 – 已创建的新项目 – 门户视图

##### 3. 单击“添加新设备”，从设备列表中选择“控制器”。展开SIMATIC S7-1200 | CPU | CPU1211C AC/DC/Rly，然后根据您打算使用的PLC选择其订货号，例如6ES7 211-1BE40-0XB0：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_373_1.png)

图10.6 – 选择您的控制器（PLC）

- 4. 单击**添加**，您应该会看到以下屏幕：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_374_0.png)

图10.7 – PLC已添加到项目中

- 5. 展开**程序块**，双击**主程序（OB1）**以创建编写程序的环境，如下所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_374_1.png)

##### 图10.8 – 梯形图编程环境

6. 编写程序时，PLC上不连接物理按钮。将使用存储器位，如下所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_375_0.png)

##### 图10.9 – 使用M0.0和M0.1作为存储器位的启动和停止梯形图程序

7. 右键单击每个变量，单击“重命名变量”：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_376_0.png)

##### 图10.10 – 重命名变量

8. 输入新名称并单击“更改”。对程序中所有变量名称执行此操作，得到如下结果：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_376_1.png)

##### 图10.11 – 所有变量均已重命名

9. 单击“保存项目”以保存项目。

10. 单击“启动仿真器”：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_377_0.png)

图10.12 – 启动仿真器时出现的消息

11. 单击“确定”。

12. 按照截图所示选择PN/IE和PLCSIM1200/1500，然后点击“开始搜索”：

图10.13 – 扩展下载到设备对话框

13. 选择CPUcommon，点击“加载”以将程序加载到虚拟PLC：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_379_0.png)

图10.14 – 扩展下载到设备对话框 – 选择PLCSIM S7-1200/S7-1500作为PG/PC接口

14. 在“加载预览”对话框中单击“加载”：

图10.15 – 加载预览

15. 在“加载结果”对话框中勾选“全部启动”，并单击“完成”：

图10.16 – 加载结果

16. PLC程序已成功加载到虚拟PLC。您应该会在屏幕上看到您的虚拟PLC，如下面的截图所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_381_0.png)

图10.17 – 屏幕上的虚拟PLC

您可以最小化虚拟PLC，以便它不会妨碍您的屏幕。

现在让我们向项目中添加一个HMI：

- 1. 单击“视图”|“转至门户视图”：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_382_0.png)

图10.18 – 视图 | 转至门户视图

2. 单击“添加新设备”，然后从设备列表中选择“HMI”。

3. 展开SIMATIC Basic Panel | 4” Display | KTP400 Basic，然后根据您打算使用的HMI选择其订货号，例如6AV2 123-2DB03-0AX0。单击“添加”：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_383_0.png)

图10.19 – 选择您的HMI

- 4. 单击**取消**以取消HMI设备向导，因为我们希望不使用向导来完成此操作：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_383_1.png)

图10.20 – HMI设备向导对话框

###### 注意

使用向导可以更快地创建HMI画面。但是，像我们这里所做的那样从头开始创建更为专业。

5. 单击“确定”：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_384_0.png)

图10.21 – 取消HMI设备向导的警告消息

一旦您单击“确定”，您应该会看到以下屏幕：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_385_0.png)

图10.22 – HMI已添加到项目中

###### 注意

HMI_1 (KTP400)现已添加到我们的项目中。现在项目中有两个文件夹（PLC_1和HMI_1）。

- 6. 双击屏幕左侧项目文件夹中的“设备和网络”：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_386_0.png)

图10.23 — 设备和网络

- 7. 将网络端口从PLC拖到HMI，得到如下结果：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_386_1.png)

图10.24 — 连接PLC和HMI（Profinet）

现在让我们设计一个画面，即GUI，操作员可以用它来启动或停止电机，并通过指示器显示电机的状态。

- 8. 在左侧，展开HMI文件夹，**HMI_1 | 画面 |** 双击**Screen_1**：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_387_0.png)

图10.25 — HMI画面

- 9. 通过将一个按钮从**元素**选项卡拖到画面上来插入按钮，如下所示，并将其名称更改为`Start`：



![](img/b10cb271ed113c7ba5bfded0a75d13e0_388_0.png)

Figure 10.26 — Inserting a button

+   10. Insert one more button and change the name to `Stop` as shown here:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_388_1.png)

Figure 10.27 — Inserting an additional button

Let’s change the appearance of our button.

11. You can select a button and change its appearance through the Properties tab as shown here:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_389_0.png)

Figure 10.28 – Changing the appearance of a button

Note

In the preceding screenshot, Solid was selected as Fill pattern and a green color was selected. You can try to create something similar.

Let’s now add functionality to our button.

12. Select the Start button and click the Events tab:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_390_0.png)

Figure 10.29 — Events tab

As you can see, various options are available (**Click, Press, Release,** and so on). You can specify a function you want the button to carry out when clicked, pressed, released, and so on. Here, we are going to use **Press**.

+   13. Select **Press** and click the **Add function** drop-down list:

Figure 10.30 – Add function drop-down arrow

14. Search for SetBitWhileKeyPressed and select it:

Figure 10.31 – SetBitWhileKeyPressed selected

15. Click inside the Tag (Input/output) box and click ...:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_392_0.png)

Figure 10.32 – Tag (input/output) options

16. Expand PLC tags and double-click Default tag table(31) to get what it looks like in the following screenshot.

17. Double-click Start:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_393_0.png)

Figure 10.33 — Default tag table (31)

You should get the following:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_393_1.png)

Figure 10.34 — PLC tag (start) selected for the Start button

+   18. Select the **Stop** pushbutton, click **Events**, and repeat *steps 13 to 16*.

+   19. Double-click **Stop**:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_394_0.png)

Figure 10.35 – PLC tag (stop) selected for the Stop button

We have successfully added functionality to our buttons.

Let’s now draw a rectangle to represent our motor:

+   1. Insert a rectangle by dragging it from the **Basic objects** tab on the right-hand side to the screen as shown here:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_395_0.png)

Figure 10.36 — Rectangle added to the screen

+   2. Click **Animations** and select **Dynamize colors and flashing**:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_395_1.png)

Figure 10.37 — Animation types

![](img/b10cb271ed113c7ba5bfded0a75d13e0_397_0.png)

Figure 10.39 – PLC tag list

5. Double-click to add the range 0 and 1 and change the background color to red and green respectively, as shown in the screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_397_1.png)

Figure 10.40 – Range 0 and 1 added for the motor tag

+   6. Click **Save project**, click on your screen to deselect the rectangle, and click **Start simulator | OK:**

![](img/b10cb271ed113c7ba5bfded0a75d13e0_398_0.png)

Figure 10.41 – Start simulation message

You should now see the simulator (**RT Simulator**) as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_399_0.png)

Figure 10.42 – HMI simulator

+   7. On the left-hand side, expand your PLC folder (PLC 1) and expand **Program blocks**. Double-click **Main [OB1]** to view your PLC program:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_399_1.png)

Figure 10.43 – PLC ladder logic program for the start and stop program

+   8. Click **Monitoring ON/OFF** to get the following:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_400_0.png)

Figure 10.44 – Monitoring the ladder logic program

+   9. Arrange your windows to show both the PLC ladder logic program and the HMI screen, as shown here:

11. Press and release the Stop button:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_402_0.png)

Monitoring ON/OFF

Figure 10.47 – Simulation result when the Stop button is pressed

12. Click Monitoring ON/OFF to stop the PLC program simulation:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_403_0.png)

##### Figure 10.48 – Go offline confirmation message

+   13. Click **Yes** to go offline.

+   14. Close the HMI simulator. Click **Save project** to save changes to your project.

Let’s advance our knowledge more by learning how to use switches and graphics:

+   1. Write this program in **Network 2** and give it an appropriate address (e.g. M0.1, Q0.2) and tag name as shown:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_404_0.png)

Figure 10.49 – Program to start and stop a mixer-motor

+   2. Click **Download to device** since our simulator is already running.
+   3. Click **Load | Finish**.
+   4. At the left-hand side, expand the HMI folder (**HMI_1**) and double-click **screen_1**.
+   5. Insert a switch by dragging it from the **Elements** tab at the right-hand side to the screen you are currently developing and resize as required. Your screen should look similar to the following:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_405_0.png)

Figure 10.50 – Switch inserted and resized on the screen

6. Click on Properties and click General, as shown in the following screenshot. For Mode, select Switch with text:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_405_1.png)

##### Figure 10.51 – General tab when the switch is selected

+   7. In the label section, type some text for **ON** and some text for **OFF**, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_406_0.png)

Figure 10.52 – General tab when switch with text is selected in the format drop down list for Mode

+   8. Click … on Tag, expand **PLC tags**, and double-click **Default tag table**, as shown here:

##### Figure 10.53 – Default tag table

9. Double-click switch1 to select switch1 and have your screen look as follows:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_407_0.png)

![](img/b10cb271ed113c7ba5bfded0a75d13e0_407_1.png)

Figure 10.54 – switch1 selected as the tag

10. Click Appearance and change the text color to red as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_408_0.png)

Figure 10.55 – Text color changed to red

11. Insert a motor by dragging it from the Graphics tab at the right-hand side onto the screen and resize as required. You will have to go to Graphics | WinCC graphics | Automation equipment | Motors | True color. You should see a list of motor graphics below your selection, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_409_0.png)

Figure 10.56 – List of motor graphics

12. Now drag the motor to your screen to get the following:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_409_1.png)

Figure 10.57 – Graphic of the motor on the screen

13. Insert a circle by dragging one from the Basic objects tab and carefully placing it on the motor, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_410_0.png)

Figure 10.58 – Circle placed on the motor graphic

14. Select the circle on the motor and click Animations | Dynamize color and flashing, then follow the steps you took for the rectangle, but you should now see mixer-motor when you double-click Default tag table[31], as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_411_0.png)

Figure 10.59 – Default tag table showing mixer-motor in the PLC tag list

15. Double-click mixer-motor to get the following:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_411_1.png)

Figure 10.60 – mixer-motor selected

16. Double-click to add the range 0 and 1 and change the background color to red and green respectively, as shown in the following screenshot:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_412_0.png)

Figure 10.61 — Range and color selected

17. Save the project, then click on your screen to deselect the circle. Click **Start simulator** and click **OK** when the start simulation message appears:



![](img/b10cb271ed113c7ba5bfded0a75d13e0_413_0.png)

##### 图 10.62 – HMI 模拟器

+   18. 展开 PLC | 程序块并双击 Main_0B1。

+   19. 点击 Monitoring ON/OFF 以获得以下内容：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_413_1.png)

##### 图 10.63 – 梯形逻辑的监视模式

20. 现在调整你的屏幕以同时显示 PLC 梯形模拟和 HMI 模拟器，如下所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_414_0.png)

##### 图 10.64 – HMI 模拟器和 PLC 梯形逻辑处于监视模式

21. 点击开关并检查结果。开关应显示 Switch is ON，而电机上的圆圈应变为绿色，如下所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_415_0.png)

图 10.65 — 点击开关时的仿真结果

+   22. 现在再次点击开关。开关应显示 **Switch is OFF**，而电机上的圆圈应变为红色，如以下屏幕截图所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_415_1.png)

图 10.67 – 使用以太网电缆将 PC（笔记本电脑）连接到 PLC（S7 1200）。（此图像的版权归 Showlight Technologies LTD 所有。www.showlight.com.ng）

2. 在屏幕左侧，展开 PLC 文件夹，进入程序块，并双击 Main [OB1] 以获取你编写的程序。双击设备配置：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_417_0.png)

图 10.68 – 梯形逻辑程序

3. 点击 属性 | 通用 | PROFINET 接口 [X1]：

图 10.69 – 设备配置（PLC_1）

4. 向下滚动并输入 IP 地址和适当的子网掩码：

编译 下载到设备

图 10.70 – PLC_1 的 IP 地址和子网掩码框

5. 点击 保存，点击 编译，并点击 下载到设备：

###### 注意

在点击 下载到设备 之前，请确保 PLCSIM 未在运行。

![](img/b10cb271ed113c7ba5bfded0a75d13e0_419_0.png)

图 10.71 – 扩展下载到设备对话框

6. 选择 PN/IE 作为 PG/PC 接口的类型并选择你的以太网卡作为 PG/PC 接口：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_420_0.png)

图 10.72 – 已选择的 PG/PC 接口

7. 点击 开始搜索。它应找到 PLC，如以下屏幕截图所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_420_1.png)

##### 图 10.73 – 搜索后找到的 PLC

8. 选择 PLC 并点击 加载：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_421_0.png)

##### 图 10.74 – 加载预览对话框

###### 注意

在前一个屏幕之前可能会出现其他屏幕/消息；阅读消息并相应响应。如果一切正常，它应带你进入加载预览对话框。

9. 在 加载结果 对话框中标记 开始全部。点击 完成：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_422_0.png)

图 10.75 — 加载结果对话框

+   10. 如果加载成功，你将看到类似以下屏幕，显示 **Message** 部分中加载已完成：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_422_1.png)

图 10.76 – 加载后屏幕显示 Message 部分中加载完成

恭喜，你已成功为 PLC 分配了 IP 地址并将 PLC 程序和配置加载到真实 PLC（S71200，CPU 1211C）上。

你可以通过从命令提示符 ping 分配给 PLC 的 IP 地址（192.168.0.4）来确认你的 PC 是否与 PLC 通信，如以下屏幕截图所示。如果有通信，你应会看到屏幕截图所示的回复：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_423_0.png)

图 10.77 – 192.168.0.4（分配给 PLC 的 IP 地址）的 Ping 结果

让我们现在继续下载程序到 HMI：

+   1. 通过将 24V 电源单元的 L+ 和 M 连接到 HMI 的 L+ 和 M 来为 HMI 供电，如下所示：

图 10.79 – 为 HMI 指定 IP 地址 192.168.0.5 和子网掩码 255.255.255.0

4. 点击 保存项目 并点击 编译。你应得到以下内容：

##### 图 10.80 – 保存并编译后屏幕

5. 你可以在命令提示符中 ping 你的 HMI 的 IP 地址，就像我们为 PLC 所做的那样。如果你的 HMI 与你的 PC 之间有通信，你应会得到以下屏幕截图所示的回复：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_426_0.png)

##### 图 10.81 – 192.168.0.5（分配给 HMI 的 IP 地址）的 Ping 结果

6. 双击 screen_1 以查看你设计的屏幕，如下所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_426_1.png)

图 10.82 — 在上一节中开发的 HMI 屏幕

+   7. 点击 **下载到设备**。在 **PG/PC 接口类型** 框中选择 **PN/IE**。同时，在 **PG/PC 接口** 框中选择你的以太网卡，点击 **开始搜索** 以搜索已连接的 HMI，选择找到的 HMI 并点击 **加载**：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_427_0.png)

图 10.83 — 显示找到的 HMI 的扩展下载到设备对话框

+   8. 标记 **覆盖全部** 和 **适配**，然后点击 **加载**：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_428_0.png)

图 10.84 – 加载预览对话框

9. 你可能会得到以下错误：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_428_1.png)

图 10.85 – 带有错误的加载结果

如果看到与前面类似的错误，并且 HMI 面板显示以下图像中所示的错误，只需通过选择 **设置 | 传输设置** 并将数字签名选择为 **关闭** 来停用 HMI 面板上的数字签名，然后点击 **传输** 返回到 HMI 面板上的传输屏幕。你可以简单地关闭 PC 上的 **加载结果** 错误并点击 **下载到设备**，选择找到的 HMI，点击 **加载**，标记 **覆盖全部**，然后点击 **加载**。

![](img/b10cb271ed113c7ba5bfded0a75d13e0_429_0.png)

图 10.86 – 传输期间 HMI 面板显示错误。（此图像的版权归 Showlight Technologies LTD 所有。 [www.showlight.com.ng](http://www.showlight.com.ng)）

+   10. 如果一切正常，它将开始加载并显示 **加载完成** 消息，如以下屏幕截图所示：

##### 图 10.87 – 加载完成

下图显示了已加载设计屏幕的 HMI 面板：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_431_0.png)

图 10.88 – 已加载程序的 HMI 面板（KTP400）。（此图像的版权归 Showlight Technologies LTD 所有。www.showlight.com.ng）

+   11. 现在从 PC 上拔下以太网电缆并将其连接到 PLC，以便以太网电缆连接在 PLC 和 HMI 之间：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_432_0.png)

图 10.89 – HMI 和 PLC 通过以太网电缆连接。（此图像的版权归 Showlight Technologies LTD 所有。www.showlight.com.ng）

让我们现在进行测试以检查一切是否正常：

+   1. 在 HMI 上按下 Start。红色矩形应变为绿色，PLC 上的 I0.0 应打开，如以下照片所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_433_0.png)

图 10.90 – 红色矩形变为绿色。I0.0 打开。（此图像的版权归 Showlight Technologies LTD 所有。www.showlight.com.ng）

+   2. 在 HMI 上按下 Stop。绿色矩形应变为红色，PLC 上的 I0.0 应关闭。

很好，我们现在已成功将程序下载到我们的 PLC 和 HMI，并且已在它们之间建立了通信。现在，操作员可以按下 HMI 面板上的 Start 或 Stop 来启动或停止电机。混合电机也可以使用 HMI 面板打开或关闭，我们可以看到电机和混合电机的状态，红色表示已停止，绿色表示正在运行。

##### 摘要

恭喜！你已成功完成本书的另一章节。干得好！HMI 与 PLC 结合在各个行业中使用，以向机器发出命令并获取机器状态的反馈。本章的动手/实践部分展示了如何使用与 PLC 接口的 HMI 来控制机器并从机器获取反馈。本章还包括仿真。即使你没有真实的 PLC 和 HMI 面板可供使用，你也应该能够仿真你的 PLC 和 HMI 程序。本章的最后一节解释了如何将程序下载到真实 PLC 和 HMI。尝试使用这些工具亲自动手。

在下一章，我们将学习关于 **Supervisory Control and Data Acquisition (SCADA)** 的知识，这是一个工业自动化工程师需要了解的另一个有趣主题。不要错过！

#### 问题

以下是测试你对本章理解的问题。在尝试问题之前，请确保你已阅读并理解本章的主题：

+   1. HMI 包含 ____________ 和 ____________，允许人类操作员与机器之间进行通信。
+   2. MMI 是 ________________ 的缩写。
+   3. OIT 是 ________________ 的缩写。
+   4. UI 是 ____________ 的缩写。



