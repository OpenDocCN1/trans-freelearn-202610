

此外，在你开始连接导线之前，这里有一张 Raspberry Pi Zero 板上 GPIO 引脚的图像：

| Pin 1 3.3V |  | Pin 2 5V |
| Pin 3 GPIO2 |  | Pin 4 5V |
| Pin 5 GPIO3 |  | Pin 6 GND |
| Pin 7 GPIO4 |  | Pin 8 GPIO14 |
| Pin 9 GND |  | Pin 10 GPIO15 |
| Pin 11 GPIO17 |  | Pin 12 GPIO18 |
| Pin 13 GPIO27 |  | Pin 14 GND |
| Pin 15 GPIO22 |  | Pin 16 GPIO23 |
| Pin 17 3.3V |  | Pin 18 GPIO24 |
| Pin 19 GPIO10 |  | Pin 20 GND |
| Pin 21 GPIO9 |  | Pin 22 GPIO25 |
| Pin 23 GPIO11 |  | Pin 24 GPIO8 |
| Pin 25 GND |  | Pin 26 GPIO7 |
| Pin 27 ID_SD |  | Pin 28 ID_SC |
| Pin 29 GPIO5 |  | Pin 30 GND |
| Pin 31 GPIO6 |  | Pin 32 GPIO12 |
| Pin 33 GPIO13 |  | Pin 34 GND |
| Pin 35 GPIO19 |  | Pin 36 GPIO16 |
| Pin 37 GPIO26 |  | Pin 38 GPIO20 |
| Pin 39 GND |  | Pin 40 GPIO21 |

你需要将 Raspberry Pi Zero 上的这些 GPIO 引脚连接到 H 桥上的引脚，如下表所示：

| Raspberry Pi Zero GPIO pin | H-bridge pin |
| 4 (5V) | 1 (Enable pin) |
| 13 (GPIO 27) | 2 (Forward) |
| 15 (GPIO 22) | 7 (Backward) |
| 4 (5V) | 11 (Enable 2) |
| 38 (GPIO 6) | 10 (Forward) |
| 40 (GPIO 13) | 15 (Backward) |
| 6 (GND) | 4, 5, 12, 13 (GND) |
| 2 (5 Volts) | 16 (VCC) |
| Battery positive terminal | 8 (Vc) |
| Battery negative terminal | GND (connect to the same GND as previous GND pins) |

连接完成后，你可以测试系统。要使这一切正常工作，你需要添加一些代码，我们将在下一节中看到。

## 使用 Raspberry Pi Zero 以编程方式控制你的移动平台

现在你的基本电机控制器功能已经运行，你需要将两个电机控制器都连接到 Raspberry Pi Zero。本节将介绍这一点，并向你展示如何以编程方式控制整个平台。

你将在最初尝试控制电机时使用 Python。用 Python 编码、运行和调试代码非常简单。你要创建的第一个 Python 程序显示在以下屏幕截图中：

![](img/324d126a7e2da696837a3e066326bd61_99_0.png)

按照以下步骤创建此程序：

要创建此程序，在你的主目录中创建一个名为 dcmotor 的目录，输入 mkdir dcmotor，然后输入 cd dcmotor。现在，通过输入 emacs dcmotor.py 打开文件（如果你使用不同的编辑器，请新建一个名为 dcmotor.py 的文件）。

现在，输入程序。让我们逐步讲解程序：

- #!/usr/bin/python: 这一行让你可以在不输入 python 的情况下运行程序，而是在文件名之前。你将在这些说明的最后学习如何做到这一点。
- import RPi.GPIO as io: 这让你可以导入 RPi 库，该库允许你控制 GPIO 引脚。
- io.setmode(io.BCM): 这将 GPIO 引脚的规范模式设置为 Broadcom SOC 通道（BCM）编号。这意味着你将指定要控制的引脚的 GPIO 编号，而不是实际的物理引脚值。
- in1_pin1 = 27: 这将值 27 分配给 in1_pin1 变量。
- in2_pin1 = 22: 这将值 22 分配给 in1_pin1 变量。
- in1_pin2 = 20: 这将值 20 分配给 in1_pin1 变量。
- in2_pin2 = 21: 这将值 21 分配给 in1_pin1 变量。
- io.setup(in1_pin1, io.OUT): 这将 GPIO 引脚 27 设置为输出控制。
- io.setup(in2_pin1, io.OUT): 这将 GPIO 引脚 22 设置为输出控制。
- io.setup(in1_pin2, io.OUT): 这将 GPIO 引脚 20 设置为输出控制。
- io.setup(in2_pin2, io.OUT): 这将 GPIO 引脚 21 设置为输出控制。
- def forward(): 这定义了 forward 函数。你需要打开 GPIO 27 和 GPIO 20，并关闭 GPIO 22 和 GPIO 21。
- io.output(in1_pin1, True): 在 in1_pin1 上输出 3.3 伏信号（这是 GPIO 27）。
- io.output(in2_pin1, False): 在 in2_pin1 上输出 0 伏（这是 GPIO 22）。
- io.output(in1_pin2, True): 在 in1_pin2 上输出高电压（这是 GPIO 20）。
- io.output(in2_pin2, False): 在 in2_pin2 上输出 0 伏（这是 GPIO 21）。
- def reverse(): 这定义了 reverse 函数。你将打开 GPIO 22 和 GPIO 21，并关闭 GPIO 27 和 GPIO 20。
- io.output(in1_pin1, False): 在 in1_pin1 上输出 0 伏（这是 GPIO 27）。
- io.output(in2_pin1, True): 在 in2_pin1 上输出高电压（这是 GPIO 22）。
- io.output(in1_pin2, False): 在 in1_pin2 上输出 0 伏（这是 GPIO 20）。
- io.output(in2_pin2, True): 在 in2_pin2 上输出高电压（这是 GPIO 21）。
- def stop(): 这定义了 stop 函数。你将 GPIO 22、GPIO 21、GPIO 27 和 GPIO 20 的电平设置为 0。
- io.output(in1_pin1, False): 在 in1_pin1 上输出 0 伏（这是 GPIO 27）。
- io.output(in2_pin1, False): 在 in2_pin1 上输出 0 伏（这是 GPIO 22）。
- io.output(in1_pin2, False): 在 in1_pin2 上输出 0 伏（这是 GPIO 20）。
- io.output(in2_pin2, False): 在 in2_pin2 上输出 0 伏（这是 GPIO 21）。
- while True: 这会反复循环。你可以通过按 Ctrl + C 来停止程序。
- cmd = raw_input("Enter f (forward) or r (reverse) or s (stop): "): 输入一个字符，表示你希望机器人执行的操作。
- direction = cmd[0]: 只取输入的第一个字符。
- if direction == "f": 如果方向是 "f"，则执行下一条语句。
- forward(): 执行 forward 函数。
- if direction == "r": 如果方向是 "f"，则执行下一条语句。
- reverse(): 执行 reverse 函数。
- if direction == "s": 如果方向是 "f"，则执行下一条语句。
- stop(): 执行 stop 函数。

![](img/324d126a7e2da696837a3e066326bd61_103_0.png)

以下是你刚刚添加的代码行的解释：

- io.setup(in2_pin1, io.OUT): 这将 GPIO 27 设置为输出。
- p1 = io.PWM(in1_pin1, 50): 而不是简单的开或关设置，此 PWM 设置允许程序员设置脉冲的相对宽度。这在 GPIO 27 上初始化此功能。
- p1.start(0): 这以 0% 的脉冲宽度，即关闭，启动 p1、GPIO 27 上的脉冲。
- io.setup(in2_pin1, io.OUT): 这将 GPIO 22 设置为输出。
- p2 = io.PWM(in2_pin1, 50): 这在 GPIO 22 上初始化此功能。
- p2.start(0): 这以 0% 的脉冲宽度，即关闭，启动 p2、GPIO 22 上的脉冲。
- io.setup(in1_pin2, io.OUT): 这将 GPIO 20 设置为输出。
- p3 = io.PWM(in1_pin2, 50): 这在 GPIO 20 上初始化此功能。
- p3.start(0): 这以 0% 的脉冲宽度，即关闭，启动 p3、GPIO 20 上的脉冲。
- io.setup(in2_pin2, io.OUT): 这将 GPIO 21 设置为输出。
- p4 = io.PWM(in2_pin2, 50): 这在 GPIO 21 上初始化此功能。
- p4.start(0): 这以 0% 的脉冲宽度，即关闭，启动 p3、GPIO 21 上的脉冲。
- def forward(50): 此函数通过将脉冲宽度设置为 50% 的前进方向来使单元向前移动。
- p1.start(50): 这将 p1（GPIO 27）的值设置为 50% 开和 50% 关。这应导致电机以半速向前运行。
- p2.start(0): 这将 p2（GPIO 22）的值设置为 0%。这有效地关闭了该引脚。
- p3.start(50): 这将 p3（GPIO 20）的值设置为 50% 开和 50% 关。这应导致电机以半速向前运行。
- p4.start(0): 这将 p4（GPIO21）的值设置为 0%。这有效地关闭了该引脚。
- def reverse(50): 此函数通过将脉冲宽度设置为 50% 的反向来使单元反向移动。
- p1.start(0): 这将 p1（GPIO 27）的值设置为 0%。这有效地关闭了该引脚。
- p2.start(50): 这将 p2（GPIO 22）的值设置为 50% 开和 50% 关。这应导致电机以半速反向运行。
- p3.start(0): 这将 p3（GPIO 20）的值设置为 0%。这有效地关闭了该引脚。
- p4.start(50): 这将 p4（GPIO21）的值设置为 50% 开和 50% 关。这应导致电机以半速反向运行。
- def stop(): 此函数将所有 PWM 信号设置为 0%，有效地停止电机。
- p1.start(0): 这将 p1（GPIO 27）的值设置为 0%。这有效地关闭了该引脚。

