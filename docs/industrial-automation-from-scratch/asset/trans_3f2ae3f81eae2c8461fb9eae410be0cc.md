

以下是从 ISA5.1《Instrumentation Symbols and Identification》中提取的部分 ISA 符号：

| ISA symbol | Description |
|---|---|
| ○ | 这用于表示安装在现场的设备或仪器，例如变送器、传感器或探测器。示例：TT 现场安装的温度变送器。PT 现场安装的压力变送器。 |
| — | 这用于表示安装在现场的设备或仪器，例如变送器、传感器或探测器。示例：LIC 101 液位指示控制器。FIC 100 流量指示控制器。 |
| --- | 这用于表示位于主面板后方（不可见）的设备或仪器。 |
| = | 这用于表示安装或安装在次要或辅助面板正面（可见）的设备或仪器。 |

Table 12.1 – ISA symbols for devices and instruments

Note

单个设备和测量仪器，如变送器、传感器和探测器，均由一个圆圈表示。

下表显示了 PLC 或 DCS 的 ISA 符号：

| ISA symbol | Description |
|---|---|
| [Square with diamond inside] | 这用于表示安装在现场的 PLC。 |
| [Square with diamond and horizontal solid line] | 这用于表示位于主面板正面的 PLC（面板正面可见）。 |
| [Square with diamond and horizontal dashed line] | 这用于表示位于主面板后方的 PLC（不可见）。 |
| [Square with diamond and horizontal solid line through the center] | 这用于表示安装在次要或辅助面板正面（可见）的 PLC。 |

Table 12.2 – ISA symbols for a PLC or DCS

# Note

PLC 或 DCS 由一个正方形和内部的菱形表示。

下表显示了执行显示和控制功能的设备的 ISA 符号：

| ISA symbol | Description |
|---|---|
|  | 该符号用于表示现场安装的既显示读数又执行某种控制功能的设备。 |
|  | 这用于表示在主面板正面（面板正面可见）既显示读数又执行某种控制功能的设备。 |
|  | 这用于表示在主面板后方（不可见）既显示读数又执行某种控制功能的设备。 |
|  | 这用于表示在次要或辅助面板正面（可见）既显示读数又执行某种控制功能的设备。 |

Table 12.3 – Display and control instruments

Note

既显示又执行某种控制功能的仪器由内部带圆圈的正方形表示。

下表显示了计算机系统的 ISAs 符号：

| ISA symbol | Description |
|---|---|
| ⬡ | 这用于表示安装在现场的计算机系统。 |
| ⬡ with a single horizontal line | 这用于表示位于主面板正面的计算机系统（面板正面可见）。 |
| ⬡ with a dashed horizontal line | 这用于表示位于主面板后方的计算机系统（不可见）。 |
| ⬡ with a double horizontal line | 这用于表示安装在次要或辅助面板正面的计算机系统（可见）。 |

Table 12.4 – ISA symbols for a computer system

# Note

执行某种控制功能的计算机系统由六边形表示。

*Table 12.1* 至 *Table 12.4* 中的 ISA 符号可在此处总结，以便更容易理解：

- 没有线穿过中心的形状表示位于现场的仪器或设备。
- 穿过中心有一条水平实线的形状表示仪器或设备位于主面板正面（面板正面可见）。
- 穿过中心有一条虚线的形状表示仪器或功能位于主面板后方（不可见）。
- 穿过中心有一条双线的形状表示仪器或功能位于次要或辅助面板正面（可见）。

以下是从 *ISA5.1*、*Instrumentation symbols and identification* 中提取的其他 ISA 符号：

# Table 12.5 – Other ISA symbols

可在 *ISA5.1, Instrumentation symbols and identification* 文档中找到更多信息。

我们刚刚学习了 ISA 符号。这一知识将帮助我们解读 P&ID 图。学习更多内容可使用 *ISA5.1, Instrumentation symbols and identification* 文档以及网上提供的其他资源。

在以下部分，您将学习各种量（温度、压力、液位和流量）的测量及其相关变送器。

# Temperature measurement and transmitters

温度是衡量某物热度或冷度的指标。温度测量是过程工业中的一项重要任务，可借助传感器和变送器实现。常见的传感器有 **resistant temperature detectors (RTDs)** 和 **thermocouples.**

RTD 是一种随温度变化电阻也会变化的器件。其温度范围为 -260 至 850 摄氏度。其灵敏度优于热电偶，但价格更贵。按电阻元件材料分类的 RTD 传感器类型有 Pt100、Pt1000、Ni120 和 Cu100。字母代表元件的材料，而数字代表 0 摄氏度时的电阻。Pt100 是最常见的 RTD。电阻元件为铂，在 0 摄氏度时电阻为 100 欧姆。

RTD 温度传感器需要 RTD 变送器来产生标准信号输出（例如，4 至 20 mA），以便随温度变化的电阻。图 12.6 显示了 PT100 的 RTD 温度变送器。温度范围为 0 至 300 摄氏度，模拟信号输出为 4 至 20 mA。这意味着变送器将在 0 摄氏度时产生 4 mA，在 300 摄氏度时产生 20 mA。

![](img/b10cb271ed113c7ba5bfded0a75d13e0_506_0.png)

Figure 12.6 – An RTD temperature transmitter for PT100 (Credit: Showlight Technologies Ltd.: www.showlight.com.ng)

图 12.5 显示了 RTD 温度传感器和 RTD 变送器的接线方式。

热电偶由两根不同金属的导线在一端连接形成传感接点。会产生电压（以 mV 计），并随温度变化而变化。其温度范围为 -270 至 1800 摄氏度。现有的热电偶温度传感器包括 Type K、Type J、type T、Type E、Type N 和 Type S。每种类型都有一个温度范围。Type K 是最常见的热电偶温度传感器，温度范围为 -270 至 1,260 摄氏度。

热电偶温度传感器需要热电偶温度变送器来产生标准信号输出（例如，4 至 20 mA 或 0 至 10 V），因为随温度变化产生的 mV 会发生变化。图 12.7 显示了 Type K 热电偶的热电偶温度变送器。温度范围为 0 至 400 摄氏度，模拟信号输出为 4 至 20 mA。这意味着变送器将在 0 摄氏度时产生 4 mA，在 400 摄氏度时产生 20 mA。

![](img/b10cb271ed113c7ba5bfded0a75d13e0_507_0.png)

Figure 12.7 – A thermocouple temperature transmitter for a Type K thermocouple sensor (Credit: Showlight Technologies Ltd.: www.showlight.com.ng)

您刚刚学习了温度测量和变送器。让我们继续下一部分学习压力测量和变送器。

# Pressure measurement and transmitters

压力可定义为单位面积上施加的力（F/A）。工业中的工艺需要准确可靠的压力测量以确保安全操作和生产优质产品。压力的影响包括位置移动、电阻变化或其他可测量的物理变化。常见的压力传感器采用波登管、膜片、波纹管、力平衡或可变电容等结构。

需要压力变送器来根据传感器所基于的压力效应产生标准信号（4 至 20 mA 或 0 至 10 V）。大多数情况下，压力变送器自带内置传感器并作为单个包装出售，如下图所示：

