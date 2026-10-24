

## 添加超声波传感器

你刚刚构建的基础电路是一个很好的开始。现在你将把一个更复杂的设备——超声波传感器——与 Raspberry Pi Zero 接口。以下是你将添加的超声波传感器的图片：

![](img/324d126a7e2da696837a3e066326bd61_77_0.png)

该设备是 HC-SR04，可在大多数在线电子零售商处买到。现在让我们将设备连接到 Raspberry Pi Zero。为此，首先让我们看看 Raspberry Pi Zero 上 GPIO 引脚的布局：

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

这两种电阻的组合将电压降低到所需水平。
以下是面包板上的电路图：

![](img/324d126a7e2da696837a3e066326bd61_79_0.png)

最后，如图所示将传感器连接到 Raspberry Pi Zero：

![](img/324d126a7e2da696837a3e066326bd61_79_1.png)

现在设备已连接，你需要一些代码来读取值，确保其稳定（产生稳定的测量值），然后将其转换为距离。

## Raspberry Pi Zero 和超声波传感器代码

以下是与超声波传感器通信的 Python 代码：

```
#!/usr/bin/python
import RPi.GPIO as io
import time

io.setmode(io.BCM)

trig = 23
echo = 24

io.setup(trig,io.OUT)
io.setup(echo,io.IN)

io.setup(trig, False)
time.sleep(1)
io.output(trig, True)
time.sleep(.00001)
io.output(trig, False)

while io.input(echo) == 0:
    start = time.time()

while io.input(echo) == 1:
    end = time.time()

duration = end - start
distance = duration * 17150
distance = round(distance, 2)
print "Distance: ", distance, " cm"
io.cleanup()
```

以下是代码的解释：

- #!/usr/bin/python: 这一行让你可以在不输入 python 的情况下运行此程序。你将在这些说明的末尾学习如何做到这一点。
- `import RPi.GPIO as io`: 这让你可以导入 RPi 库，这将允许你控制 GPIO 引脚。
- `import time`: time 库提供了多个基于时间的功能。在这种情况下，你将使用它来暂停程序一段时间并测量一定的时间延迟。
- `io.setmode(io.BCM)`: 这将 GPIO 引脚的规范模式设置为 Broadcom SOC 通道号（BCM）。这意味着你将指定要控制的 GPIO 引脚号，而不是实际的物理引脚值。
- `trig = 23`: 这将值 23 分配给 trig 变量。
- `echo = 24`: 这将值 24 分配给 echo 变量。
- `io.setup(trig, io.OUT)`: 这将 GPIO 引脚 23 设置为输出。
- `io.setup(echo, io.IN)`: 这将 GPIO 引脚 24 设置为输入。
- `io.setup(trig, False)`: 这将向 trig 线、引脚 23 输出零。
- `time.sleep(1)`: 此命令将使程序暂停一秒。
- `io.output(trig, True)`: 这将向 trig 线、引脚 23 输出一个，5 伏。
- `time.sleep(.00001)`: 这将使程序等待大约 10 微秒。
- `io.output(trig, False)`: 这将向 trig 线、引脚 23 输出零。
- `while io.input(echo) == 0:`: 当引脚 24 的输入为零时。
- `start = time.time()`: 持续重置开始时间。当引脚 24 变高时，start 将保存引脚 24 的值为低时的最新时间。
- `while io.input(echo) == 1:`: 当输入引脚 24 为 1 时执行以下命令集。
- `end = time.time()`: 持续重置结束时间。当引脚 24 变低时，end 将保存引脚 24 的值为高时的最新时间。
- `duration = end - start`: duration 现在将保存结束时间和开始时间之间的时间距离。
- `distance = duration * 17150`: distance 将持续时间转换为距离值。
- `distance = round(distance, 2)`: 将距离四舍五入到小数点后两位。
- `print "Distance: ", distance, " cm"`: 打印出距离。
- `io.cleanup()`: 重置接口。

## 在 Raspberry Pi Zero 上访问 GPIO 引脚

现在你应该保存并运行程序并获得结果，如以下屏幕截图所示：

![](img/324d126a7e2da696837a3e066326bd61_82_0.png)

现在你可以用 Raspberry Pi Zero 感知距离！

## 将数字罗盘连接到 Raspberry Pi Zero

现在你已经为 Raspberry Pi Zero 创建了一些非常基础的连接，打开和关闭线路并感知高或低信号。现在让我们探索 Raspberry Pi Zero 上可用的其他接口之一，I2C 接口。为此，你将连接一个使用这种接口的数字罗盘。

> 有关 I2C 接口的更多信息，请参阅 `http://www.robot-electronics.co.uk/i2c-tutorial`。

有几种芯片提供数字罗盘功能；其中最常见的是 **HMC5883L**，一种 *3 轴* 数字罗盘芯片。该芯片由多家公司封装在模块中，但几乎所有模块的结果都具有相似的接口。

以下是 **GY-271 HMC5883L** 的图片，这是一款三轴罗盘磁力计传感器模块，可在多家在线零售商处买到：

![](img/324d126a7e2da696837a3e066326bd61_83_0.png)

这种类型的数字罗盘使用磁传感器来测量地球的磁场。这些传感器的输出随后通过一组寄存器使外部世界可以访问，允许用户设置诸如采样率和连续或单次采样之类的内容。X、Y 和 Z 方向的输出也使用寄存器。

该芯片使用 **Inter IC (I2C)** 总线与 Raspberry Pi Zero 通信。Raspberry Pi 的 GPIO 接口上有三个串行总线可用。第一个是标准的 **Universal Asynchronous Receiver/Transmitter (UART)** 接口。UART 接口使用一个引脚用于 RX 或接收信号，一个引脚用于 TX 或发送信号。UART 接收字节数据并以顺序传输各个位。在目的地，第二个 UART 将位重新组装成字节。此接口仅限于与单个外部设备通信。有关 UART 接口及其工作原理的更多信息，请访问 `https://www.freebsd.org/doc/en/articles/serial-uart/`。

第二个可用的接口是 I2C 接口。I2C 接口与 UART 接口类似；它也使用两条线进行通信，在本例中为数据线和时钟线。然而，它更复杂一些，因为它允许一个主设备与多个从设备之间进行通信。它通过寻址主设备想要通信的设备来实现这一点。有关 I2C 接口的更多信息，请参阅 `http://www.robot-electronics.co.uk/i2c-tutorial`。

最后一个可用的接口是 **Serial Peripheral Interface (SPI)** 接口。与 I2C 接口一样，SPI 接口允许主设备与多个从设备之间进行通信。然而，每个从设备都使用不同的选择数据线进行选择。SPI 主设备使用 **serial clock (SCK)**、**Master Out Slave In (MOSI)**、**Master In Slave Out (MISO)** 和 **Slave Select (SS)** 线与从设备通信。SCK、MOSI 和 MISO 信号可以由从设备共享，而每个从设备都有唯一的 SS 线。有关 SPI 接口的更多信息，请参阅 http://www.corelis.com/education/SPI_Tutorial.htm。

由于你要使用的设备是 I2C 设备，罗盘模块背面将有三个连接，一个用于电源（VCC），一个用于地（GND），一个用于时钟（SCL），一个用于数据（SDA）。连接如以下图像所示：

![](img/324d126a7e2da696837a3e066326bd61_84_0.png)

