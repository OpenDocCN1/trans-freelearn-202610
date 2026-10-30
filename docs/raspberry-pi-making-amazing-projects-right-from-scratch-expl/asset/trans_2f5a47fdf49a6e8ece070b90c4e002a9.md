

## 搭建双足机器人

现在你可以使用滑块来实际控制舵机了。打开舵机 0。确认舵机 0 控制的是右下脚踝处的舵机。你也可以用它来使舵机居中。将舵机 1 设为滑块位于中间的位置。然后，拧松舵机上的舵盘，直到舵机在这个位置处于居中状态。当所有舵机都位于零位时，你的双足机器人应该看起来像这样：

![](img/631e8b200cce187082ee46b0eb1681b3_496_0.png)

### 将舵机控制器连接到树莓派

你已经检查过舵机电机控制器和舵机。现在可以将电机控制器连接到树莓派，并确保你能从树莓派控制舵机。

接下来，让我们与电机控制器通信。步骤如下：

- 1. 使用 Mini USB 线将树莓派连接到电机控制器。将线缆连接到树莓派的 USB 主机接口上，就像这样：

![](img/631e8b200cce187082ee46b0eb1681b3_497_0.png)

- 2. 从 Pololu 官网 www.pololu.com/docs/0J40/3.b 下载 Linux 代码。也许最好的办法是登录你的树莓派，然后输入 `wget http://www.pololu.com/file/download/maestro-linux-100507.tar.gz?file_id=0J315`。

- 3. 然后，使用 `mv maestro-linux-100507.tar.gz\?file_id\=0J315 maestro-linux-100507.tar.gz` 移动文件。

- 4. 输入 `tar -xvf maestro-linux-100507.tar.gz` 解压文件。这会创建一个名为 `maestro_linux` 的目录。输入 `cd maestro_linux` 进入该目录，然后输入 `ls`；你应该会看到类似这样的内容：

![](img/631e8b200cce187082ee46b0eb1681b3_498_0.png)

`README.txt` 文档中会给出关于如何安装软件的明确说明。遗憾的是，你不能在树莓派上运行 **MaestroControlCenter**；它使用的 Windows 版本不支持图形界面，但你可以通过 **UscCmd** 命令行应用程序来控制舵机。首先，输入 `./UscCmd --list`，你应该会看到以下内容：

![](img/631e8b200cce187082ee46b0eb1681b3_499_0.png)

设备识别到了你的舵机控制器。如果你直接输入 `./UscCmd`，就可以看到所有可以发送给控制器的命令：

![](img/631e8b200cce187082ee46b0eb1681b3_499_1.png)

```python
#!/usr/bin/python
import serial
import time

def setAngle(ser, channel, angle):
    minAngle = 0.0
    maxAngle = 180.0
    minTarget = 256.0
    maxTarget = 13120.0
    scaledValue = int((angle / ((maxAngle - minAngle) / (maxTarget - minTarget))) + minTarget)
    commandByte = chr(0x84)
    channelByte = chr(channel)
    lowTargetByte = chr(scaledValue & 0x7F)
    highTargetByte = chr((scaledValue >> 7) & 0x7F)
    command = commandByte + channelByte + lowTargetByte + highTargetByte
    ser.write(command)
    ser.flush()

ser = serial.Serial("/dev/ttyACM0", 9600)

# Home position

for i in range(0, 9):
    setAngle(ser, i ,90)

ser.close()
```

以下是对代码的解释：

- `#!/usr/bin/python`：第一行用于让这个 Python 文件可以从命令行直接执行。
- `import serial`：这一行导入串行库。你需要串行库来通过 USB 与设备通信。
- `def setAngle(ser, channel, angle)`：这个函数将你期望的舵机和角度设置转换为舵机电机控制器所需的串行命令。要了解用于控制舵机的代码细节，请参阅 https://www.pololu.com/docs/0J40。
- `ser = serial.Serial("/dev/ttyACM0", 9600)`：这会打开与舵机控制器之间的串口连接。
- `for i in range(0, 9)`：使用 For 循环访问全部九个舵机。
- `setAngle(ser, i, 90)`：现在，你可以将每个舵机设置到中间（初始）位置。默认是将每个舵机设置为 90 度。如果你的腿部不在中间位置，你可以通过调整每个舵机上舵盘的位置来进行校正。

要访问串口，你需要确保已安装 Python 串行库。如果没有安装，请输入 `apt-get install python-serial`。安装串行库后，你可以通过输入 `python quad.py` 来运行程序。

一旦设置好基本的初始位置，你就可以让机器人做一些动作了。我们先让双足机器人活动一下脚部。下面是 Python 代码：

![](img/631e8b200cce187082ee46b0eb1681b3_502_0.png)

在这个例子中，你使用 `setAngle` 命令来设置舵机，从而操控机器人的右脚踝。

### 小结

现在你拥有一个可以移动的机器人了！在下一章中，你将学习如何让机器人做出许多令人惊叹的事情。你将学习如何让它向前和向后行走，以及如何让它跳舞和转弯。掌握一些基础知识后，你就可以实现各种各样的动作。

## 3 双足机器人的运动

现在你的双足机器人已经能够正常运行了，你可以开始开发有趣的方式来让它移动。在本章中，你将学习：

- 如何调整舵机的位置以实现霸王龙姿态
- 机器人的基本步态
- 机器人的基本转弯

不过，在开始之前，最好先为你的双足机器人制作一个保护支架。你的机器人只有两条腿，本身就很不稳定，而在实验过程中你难免会犯错。只有两条腿的情况下，这些错误可能导致机器人摔倒，从而损坏机器人。

如果你有电子线路台钳或“辅助手”装置，它们会很有用。下面这张图展示了如何使用这种装置来制作保护支架：

![](img/631e8b200cce187082ee46b0eb1681b3_505_0.png)

如果没有，你也可以轻松地用 PVC 管或木头制作这种顶部支撑装置。实际上，只要能防止双足机器人在实验中摔倒就行。

### 基本的稳定姿态

现在你的双足机器人已经搭好，你也知道如何用 Python 编程控制舵机，可以实验一些基本姿态了。你首先要创建一个允许你单独设置每个舵机的程序，以便进行实验。下面是这个程序：

![](img/631e8b200cce187082ee46b0eb1681b3_506_0.png)

这段代码包含了第 2 章“搭建双足机器人”中的 Python `setAngle` 函数。具体细节来自 www.pololu.com 网站，它的作用就是让你可以将某个舵机设置到某个特定角度。

代码的下一部分将所有舵机设置到居中位置。最后一部分代码，即 `while 1:` 代码块，只是让用户输入舵机编号和角度，然后将命令发送到舵机控制器。

你可以使用这些角度来实现这种姿态。不过，这会限制你的活动范围，因为你的舵机将接近其单方向运动能力的极限。由于这将是机器人的起始姿态，为了获得最大的灵活性，你需要将舵机居中在这个位置。为此，请运行默认的 `robot.py`，将腿部设置到居中位置。然后，在舵机值处于 90 度角时调整舵盘，以实现这种姿态。

它应该看起来像这样：

![](img/631e8b200cce187082ee46b0eb1681b3_508_0.png)

现在你有了一个稳定的基础，可以开始编写简单的行走动作了。

### 基本行走动作

你的机器人已经准备好行走了，但首先你得让一条腿离地。当然，这很简单；只需通过改变膝关节的角度来抬起腿，你的腿就能离地。你可能还想改变脚踝前后方向的角度；这样可以让你抬起腿时不必抬得那么高。

然而，如果只改变这两个舵机，你会遇到一个问题：当你抬起腿时，机器人会摔倒。这源于一个简单的原理，叫做重心。当你的机器人静止时，它的重心是这样的：

![](img/631e8b200cce187082ee46b0eb1681b3_509_0.png)

