

![](img/b10cb271ed113c7ba5bfded0a75d13e0_278_0.png)

Figure 8.10 – Push buttons, pilot lamps, and control relay wired to a PLC (S7-1200, CPU1211C, AC/DC/Rly)

我们现在应该已经对 LD 有了基本了解。后续章节将提供进一步的知识，帮助你开始使用 LD 编程 PLC。

## Exploring the elements of LD (ladder logic) program

要使用 LD 编写程序。你需要了解构成梯形图程序的元素。以下元素构成 LD（梯形逻辑）程序：

- Rails: 这是两条垂直线，一条在最左侧（热轨），另一条在最右侧。最左侧的垂直线称为热轨，而最右侧的可以称为中性线。
- Rungs: 这些是连接两条导轨以完成电路的水平线。电功率从热轨流向中性轨。它将导轨连接到逻辑表达式。
- Inputs: 输入是表示连接到 PLC 的输入设备的常开和常闭触点符号，例如按钮、选择开关、限位开关或浮球开关。连接到 PLC 的输入设备的状态决定了程序中常开或常闭触点符号是打开还是关闭。
- Output: 输出是表示连接到 PLC 的输出设备的线圈符号，例如指示灯、继电器、接触器或电磁阀。
- Logic expression: 这是输入和输出符号的组合，用于形成所需的控制功能。
- Address notation and tag names: PLC 使用地址来标识每个输入和输出点，其中外部设备（输入和输出设备）连接到 PLC。你连接每个输入设备或输出设备的点都有一个地址，PLC 用它来识别它。PLC 还使用地址来标识内部/软件创建的设备，例如继电器、定时器和计数器。每个制造商用于标识其 PLC 中的输入/输出（输入/输出点）和软件创建设备的地址使用不同。在编程特定 PLC 制造商或 PLC 型号之前，程序员需要知道其使用的寻址或地址表示法。

例如，西门子使用 I 表示输入，Q 表示输出。我们基本上可以将输入作为 I0.0、I0.1、I0.2 等，输出作为 Q0.0、Q0.1、Q0.2、Q0.3 等。西门子中的软件创建继电器/辅助继电器为 M0.0、M0.1、M0.2 等。三菱使用 X 表示输入，Y 表示输出。我们基本上可以将输入作为 X000、X001、X002 等，输出作为 Y000、Y001、Y002 等。三菱 PLC 中的软件创建继电器/辅助继电器为 M0、M1、M2 等。然而，本书的重点将是西门子 PLC（S7 1200 CPU 1211C AC/DC/RLY）。地址表示法只是对每个输入/输出（输入或输出点）和软件创建设备，例如继电器、定时器和计数器，而标签名称是对地址的描述。

S7 1200（CPU 1211C AC/DC/RLY）有六个数字输入和四个数字输出，可以在程序中如下寻址：

- Inputs: `I0.0, I0.1, I0.2, I0.3, I0.4, I0.5`
- Outputs: `Q0.0, Q0.1, Q0.2, Q0.3`

你可以在 PLC 或 PLC 型号的手册中找到其输入/输出寻址。务必阅读你想要编程的任何 PLC 的文档/手册。

- Comments: 注释用于描述 LD 中某一横档或一组横档将执行的逻辑操作。它们通常显示在每个横档的开头。它们使程序易于理解。

下图显示了带标签的示例 LD：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_281_0.png)

Figure 8.11 – LD with labels

我们现在将学习使用 LD 编程 PLC 的规则。

## Rules for LD programming

以下规则将有助于使用 LD 编写良好的程序：

- Inputs (NO contact and NC contact) can be connected in parallel or series in LD:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_283_0.png)

Figure 8.13 – Outputs (coils) connected in parallel

- One input can be used several times in one program; that is, a single input can be used in a different rung:

![](img/b10cb271ed113c7ba5bfded0a75d13e0_284_0.png)

Figure 8.14 – Same input is used in rung 1 and rung 2

一个输出不能在一个程序中被多次使用，除非在置位/复位和锁存/解锁程序中：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_284_1.png)

Figure 8.15 – One output used twice for set/reset in a Siemens PLC program

下图显示了 Allen-Bradley PLC 程序中的一个简单的锁存和解锁程序：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_285_0.png)

Figure 8.16 – One output used twice for latch/unlatch in an Allen-Bradley PLC program

- The address of an output (coil) can be used for input (NC or NO contact). You can use the address of an output (coil) for an input (NC or NO contact), which makes the input have the same logic state (1 or 0) as the output; that is, if the output is ON (1), the input will be HIGH (1), and if the output is OFF, the input will be LOW (0).

![](img/b10cb271ed113c7ba5bfded0a75d13e0_286_0.png)

Figure 8.17 – Sample program showing output address used as input address

> The address of an input cannot be used for output. Don't make the mistake of using an input address for output (coil).

我们现在应该知道 LD 编程的规则。这些规则将在后续章节中使用 LD 编写程序时应用。我们将继续下一节学习如何下载和安装编程软件以及模拟器。

## Downloading and installing TIA Portal V13 Professional and PLCSIM

西门子提供其编程软件（TIA Portal）的 21 天免费试用。还有一个模拟器，允许你模拟程序。

因此，即使你没有真正的 PLC，你也可以编写程序并通过使用模拟器（PLCSIM）查看其运行情况。你可以免费在线下载 TIA Portal V13 Professional 和 PLCSIM 以学习 PLC 编程，或购买 TIA Portal 许可证以避免任何限制。

## Downloading TIA Portal V13 and PLCSIM

下载 TIA Portal V13 和 PLCSIM 应遵循以下步骤：

- 1. 第一步是访问下载页面。以下链接应带你到下载页面：

https://support.industry.siemens.com/cs/document/109745155/simatic-step-7-including-plcsim-v13-sp2-trial-download?dti=0&lc=en-WW

以下屏幕截图显示了前述链接的页面：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_287_0.png)

# Figure 8.18 – Download page of Siemens TIA Portal V13 Professional and PLCSIM (1)

- 2. 滚动到 **STEP 7 Professional V13 SP2** 部分，如以下屏幕截图所示，并下载同一文件夹中指示的所有文件：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_288_0.png)

# Figure 8.19 – Download page of Siemens TIA Portal V13 Professional and PLCSIM (2)

### Note

一旦你点击任何文件进行下载，就会弹出登录屏幕。如果你已有用户名和密码，则需要使用你的西门子工业在线支持用户名和密码登录；如果没有，你将必须点击 **Yes, I would like to register now**，按照说明完成注册，然后登录下载文件。

3. 此外，滚动到 **SIMATIC STEP 7 PLCSIM V13 SP2 for STEP 7 Basic and STEP 7 Professional** 并下载同一文件夹中指示的所有文件，如以下屏幕截图所示：

![](img/b10cb271ed113c7ba5bfded0a75d13e0_289_0.png)

Figure 8.20 – Download page of Siemens TIA Portal V13 Professional and PLCSIM (3)

让我们在下一节学习如何安装 TIA Portal V13。

