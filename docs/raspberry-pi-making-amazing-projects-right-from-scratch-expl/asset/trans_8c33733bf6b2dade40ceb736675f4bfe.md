
![](img/631e8b200cce187082ee46b0eb1681b3_381_0.png)

Raspberry Pi NoIR 摄像头模块；它看起来与标准型号相似。

## 一次照明体验

为了通过 Raspberry Pi GPIO 或我们的端口扩展器电路点亮灯泡或 LED 阵列，我们需要某种器件，能够驱动比 GPIO 端口单独提供的更高的电流和电压。

TIP120 达林顿晶体管是一个很好的选择，它允许我们从 GPIO 引脚开关高达 80V 和 5A 的负载。在后续的完整系统中，我们将使用 MCP23017 端口扩展器的 B 端口来控制输出，但该原理适用于我们可用的任何 GPIO 输出。

![](img/631e8b200cce187082ee46b0eb1681b3_381_1.png)

TIP120 晶体管可以廉价购买，但能够驱动大负载。

下面的电路展示了如何从 GPIO 端口输出驱动大负载。

![](img/631e8b200cce187082ee46b0eb1681b3_382_0.png)

在示例电路中，我们使用一个 GPIO 输出引脚通过 220 欧姆电阻控制晶体管的基极。当 GPIO 引脚变为高电平时，晶体管导通，使 12V 电路通过 LED 阵列流动。

在上述电路中，没有为 LED 设置限流电阻，因为它们是串联的，九个 LED 每个压降约 1.5V，对于 12V 电源来说大致合适（是的，我知道这里只画了六个 LED，但仅为示意）。请记得根据您的具体需求进行调整。该电路可以轻松驱动其他负载，例如灯泡或蜂鸣器。

> 如果您打算驱动高功率负载，可能需要将 TIP120 安装到散热器上，以消散热量并防止过热烧毁。不过，在我们之前演示的电路中，可能不需要散热器，因为我们最多只驱动几百毫瓦的功率。

## 再访精致的电灯开关

再次扩展前几章中的精致电灯开关，我们可以再次编写一个 Bash 脚本，当 PIR 检测器被触发时，该脚本将打开摄像头灯，用摄像头拍照，并通过电子邮件发送给我们。

对于以下脚本，我们假设控制 TIP120 晶体管的输出是树莓派的 GPIO17 引脚（即连接器的 D0 或引脚 11），它取代了之前设置中的 LED。来自 PIR 触发器的输入同样连接到 MCP23017 端口扩展器的 GPA0（A 端口数据引脚 0）。所有其他输入如前所述，使用 10K 电阻拉低：

```bash
#!/bin/bash

#set up the High Load GPIO pin
sudo echo 17 > /sys/class/gpio/export
sudo echo out > /sys/class/gpio/gpio17/direction

#set up port expander Port A for inputs
sudo i2cset -y 1 0x20 0x00 0xFF

#clear the output by default to switch light off
sudo echo 0 > /sys/class/gpio/gpio17/value

# loop forever
while true
do
  # read the sensor state
  SWITCH=$(sudo i2cget -y 1 0x20 0x12)

  #PIR is normally closed so pin is held high
  if [ $SWITCH != "0x01" ]
  then
    #PIR was triggered - pin taken low

    #switch on lamp driver
    sudo echo 1 > /sys/class/gpio/gpio17/value
    sleep 0.5

    #take a still image
    sudo raspistill -o -image.jpg -h 768 -w 1024 -q 25

    #email the image
    mpack -s "Security Alert Photo" test.jpg me@mydomain.com

    #switch off the lamp driver
    sudo echo 0 > /sys/class/gpio/gpio17/value
fi
#short delay
sleep 0.5
done
```

pir-camera-trigger.sh

您现在会看到，我们已经开始开发将控制家用安防系统的软件基础。

## 那是獾吗？

如果您不想在拍摄图像前照亮某个区域，可以使用**红外照明**配合兼容的摄像头。标准的树莓派摄像头模块无法与红外照明配合使用，因为它包含红外滤光片，但我们可以改用 NoIR 版摄像头模块。

树莓派 NoIR 摄像头模块与标准模块完全相同，只是没有内置红外滤光片，这意味着在红外照明的帮助下，它可以在黑暗中看到东西。这使得它既适合夜间观察獾，也适用于我们的家用安防系统。

您需要一个红外 LED 阵列或灯组，以不可见光的方式照亮您想用摄像头拍摄的区域。这些产品有多种外形和强度可供选择，您也可以使用从电子商店购买的单颗红外 LED 自行制作。

![](img/631e8b200cce187082ee46b0eb1681b3_384_0.png)

连接和驱动 LED 灯组模块的方式与上述照明灯完全相同，使用 TIP120 驱动电路。唯一不同的是，我们人眼无法看到 LED 是否点亮。

## 使用 USB 摄像头

除了使用树莓派摄像头模块，也可以使用标准的 USB 网络摄像头拍摄静态图像。但您应该知道，专用摄像头模块在图像质量方面远优于 USB 网络摄像头。不过，您可能已经在零件箱里有一个闲置的网络摄像头，为什么不试试呢？

## 安装网络摄像头

将网络摄像头连接到树莓派的 USB 端口后，您可以使用 `lsusb` 命令检查它是否已被识别：

```
```bash
$ lsusb
```
```

我使用的罗技网络摄像头在 `lsusb` 下报告如下（设备 006）：

```text
pi@raspberrypi ~ $ lsusb
Bus 001 Device 002: ID 0424:9514 Standard Microsystems Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 0424:ec00 Standard Microsystems Corp.
Bus 001 Device 006: ID 046d:08d8 Logitech, Inc. QuickCam for Notebook Deluxe
```

> > 并非所有网络摄像头都能在树莓派上工作。即使它可能被识别为 USB 设备，也可能无法与操作系统正常工作并创建视频设备（例如 `/dev/video0`）。例如，我有一款旧的廉价 Trust 网络摄像头，它显示为 USB 设备，但无法捕捉任何图像。您可以在 http://elinux.org/RPi_USB_Webcams 查看您的网络摄像头型号，以确认其是否可能与树莓派兼容。

所以，现在树莓派知道我们连接了网络摄像头设备，我们可以使用 `fswebcam` 工具来捕获图像帧。您可以在开发者网站 http://www.sanslogic.co.uk/fswebcam 上了解更多关于 fswebcam 的信息。

使用以下命令安装 `fswebcam`：

```
```bash
$ sudo apt-get install fswebcam
```
```

![](img/631e8b200cce187082ee46b0eb1681b3_385_0.png)

## 拍摄快照

您现在可以通过捕获静态图像来测试网络摄像头，运行以下命令即可：

```
$ fswebcam test.jpg
```

您应该会看到类似以下的输出：

![](img/631e8b200cce187082ee46b0eb1681b3_386_0.png)

> `fswebcam` 有许多选项，例如图像的分辨率和质量。使用命令 `fswebcam -?` 获取所有选项列表。

## 连续快照

`fswebcam` 不能拍摄视频流，但您可以将其设置为按固定间隔拍摄一系列帧。例如，要每 10 秒拍摄一张快照，可以使用以下命令：

```
$ fswebcam frame.jpg -l 10
```

一个有用的示例是，将网络摄像头设置为在后台每隔几秒拍摄一张快照（`-q` 开关让 `fswebcam` 在后台运行）。当安防系统被触发时，我们可以获取网络摄像头拍摄的最新图像，该摄像头可能正对着您的通道。

为了在本书后面构建完整系统，我们将重点使用树莓派摄像头模块，但如果您想改用 USB 网络摄像头，也可以随时用之前的示例替换代码。

您会注意到，与 `raspistill` 不同，`fswebcam` 能够在图像上叠加时间戳信息，因此您无需像之前那样担心叠加文本。请查看 `fswebcam` 命令行选项以了解更多信息。

## 多摄像头设置

您可能已经想到，树莓派只有一个摄像头模块输入。如果您想在房屋周围安装多个由运动检测器触发的摄像头，这显然会受限。

然而，我们完全可以构建独立单元，每个单元配备单独的树莓派板、PIR 探测器、摄像头模块和网络连接（使用 Wi-Fi 适配器或以太网）。

由于只需一个输入到树莓派的 GPIO 端口即可检测 PIR 运动传感器何时触发，您可以使用板载 GPIO 端口连接传感器，而非端口扩展器。树莓派将通过电子邮件发送警报，并在需要时提醒主控制器树莓派——使其成为从属传感器设备。

您可以轻松获得小型 PIR 探测器，例如下面显示的 Parallax 产品，您可以将其与摄像头模块一起安装到树莓派外壳上，构成一个独立的单元。

![](img/631e8b200cce187082ee46b0eb1681b3_387_0.png)

