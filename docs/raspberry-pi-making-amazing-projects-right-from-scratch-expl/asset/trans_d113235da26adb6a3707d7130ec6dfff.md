

## 为我们的安全系统添加摄像头

如以下图像所示，排线连接器并不算长，因此摄像头需要靠近 Raspberry Pi 放置。通过使用摄像头外壳，你实际上可以将摄像头直接安装在 Raspberry Pi 外壳顶部，如果这样对你有用。

![](img/631e8b200cce187082ee46b0eb1681b3_372_0.png)

装在外壳内的摄像头模块

## 设置摄像头模块

在我们使用摄像头模块之前，需要在 Raspberry Pi 上启用摄像头支持。为此，我们使用 `raspi-config` 工具，就像我们在旅程早期对 I2C 总线所做的那样。

- 1. 使用 SSH 从沙发上以懒人方式连接到你的 Raspberry Pi，或直接使用键盘和显示器连接。
- 2. 登录后，使用以下命令启动配置工具：

```bash
$ sudo raspi-config
```

3. 然后，选择 5 Enable Camera。

![](img/631e8b200cce187082ee46b0eb1681b3_373_0.png)

4. 然后会要求你确认是否要启用摄像头支持。

![](img/631e8b200cce187082ee46b0eb1681b3_373_1.png)

5. 选择 <Enable>。

6. 然后，选择 Finish 并重新启动 Pi 以启用摄像头设置。

![](img/631e8b200cce187082ee46b0eb1681b3_375_0.png)

Raspberry Pi 摄像头模块拍摄的测试照片

> 如果你运行 raspistill 时发现输出错误，请确保排线两端都已正确连接。另一个需要注意的地方是，有时将实际摄像头镜头组件连接到摄像头板上微小连接器的排线会松动。请确保此连接也牢固。我在将摄像头模块从我的随机测试零件盒中取出使用时，有几次遇到过这个问题。

raspistill 实用程序具有大量用于操纵其捕获图像的选项，我们稍后在捕获脚本中会使用其中的一些选项。在此期间，要查看可用选项，请在不带任何选项的情况下运行 `raspistill`，它们将被列出：

```bash
$ raspistill
```

## 做个视频明星

现在我们知道摄像头模块正在工作，我们可以尝试捕获一些视频。为此，我们将使用 `raspivid` 实用程序。以下命令将拍摄 5 秒的高清视频并将文件保存到你的 Raspberry Pi：

```bash
$ raspivid -o test.h264 -t 5000
```

你会注意到该文件名为 `test.h264`——这是因为视频以原始 **H.264** 视频流的形式捕获。不幸的是，并不是很多媒体播放器都能处理这些文件（尽管 VLC 播放器可以——它很棒，可以处理你扔给它的几乎任何东西——在你的 PC 上获取它，网址为 `www.videolan.org`）。

## 为我们的安全系统添加摄像头

如果你想在智能手机和常规媒体播放器上播放该文件，那么我们需要将其包装在容器格式中，例如 MPEG-4，并为文件添加 .mp4 扩展名。

为此，我们将使用 GPAC 软件包，它是一个开源多媒体框架。它附带一个名为 MP4Box 的实用程序，我们将使用它为我们的视频文件创建 MP4 容器：

- 1. 首先，安装 GPAC 软件包：
  
  ```bash
  $ sudo apt-get install gpac
  ```
  
- 2. 安装后，运行命令转换我们创建的测试视频：
  
  ```bash
  $ MP4Box -fps 30 -add test.h264 test.mp4
  ```

你现在应该拥有文件 test.mp4，可以将其下载并在你的 PC 或智能手机上播放。

> 另一个流行的转换工具是 ffmpeg，我在 Windows 上经常用它来转换视频文件；然而，它可能相当复杂，虽然 Raspberry Pi 有该软件包，但我实际上无法在 Pi 上正确转换。MP4Box 更加简单直接，更适合我们的需求。

## 被摄像头捕捉

因此，我们现在有了一种捕获静态图像和视频的方法，可以将其用于我们的安全系统。如果我们想让它不断运行，我们可以编写一个脚本来不断拍摄视频，但这很快就会填满我们的存储卡，而且效率也不高。因此，我们将把摄像头系统与我们之前连接的运动探测器结合起来。

在上一章中，我们创建了一个报警区域，该区域连接了几个传感器和一个连接到我们系统输入 GPA0 的运动探测器。因此，让我们编写一个脚本，以便在运动探测器被触发时拍摄视频片段：

```bash
#!/bin/bash

# set up port expander
sudo i2cset -y 1 0x20 0x00 0xFF

# loop forever
while true
```

```sh
do
    # read the GPA inputs
    GPA=$(sudo i2cget -y 1 0x20 0x12)

    # detect the zone on input 0
    if [ $GPA == "0x01" ]
    then
        #circuit normally closed so zone is OK
        #short delay
        sleep 0.5

    else
        #zone is activated so take a 20 sec video clip

        #filename will be based on current timestamp
        sDate='date +%d%m%y'
        sTime='date +%T'
        echo "Zone 1 Activate at $sDate $sTime"

        #take video clip
        raspivid -o $sDate$sTime.h264 -t 20000

        #convert to MP4
        MP4Box -fps 30 -add $sDate$sTime.h264 $sDate$sTime.mp4
    fi
done
```

## 你有新邮件

将图像存储在你的 Raspberry Pi 上其实没什么用——理想情况下，你希望图像在被捕获后立即发送给你，以便你可以在智能手机上查看它们。

一种简单、快速且可靠的方法就是直接通过电子邮件发送给自己。因此我们将为家庭安全系统添加电子邮件功能，以便图像捕获会作为消息附件立即发送到你的电子邮件地址，你可以从智能手机访问。图像随后可以从 Raspberry Pi 上删除，以防止 SD 卡空间被这些相当大的文件占用。

## 发送附件

现在我们可以在家庭安全系统上发送基本电子邮件，让我们尝试发送我们之前从摄像头拍摄的静态图像。但首先，我们需要安装另一个软件包来帮助我们完成此操作：

```
$ sudo apt-get install mpack
```

安装后，你可以使用以下命令发送我们之前拍摄的测试图像文件：

```
$ sudo mpack -s "Security Photo" test.jpg me@mydomain.com
```

我们现在拥有了所有必要元素，可以直接通过电子邮件将警报和图像从家庭安全系统发送到我们的智能手机。

## 那是在哪里拍摄的？

通常，你只需在电子邮件消息中注释说明所附图像的拍摄地点和时间，但那样就不如直接在图像上叠加一些文字那么酷了，对吧？所以让我们借助 imagemagick 来施展魔法，它是一个流行的命令行图像处理工具。使用以下行安装它：

```
$ sudo apt-get install imagemagick
```

我们现在将使用命令行拍摄我们之前拍摄的测试照片，使用 imagemagick 实用程序之一叠加一些文字，并将其保存到另一个文件：

```
$ convert test.jpg -fill red -pointsize 48 annotate +20+60 'Camera 1' annotated.jpg
```

## 为我们的安全系统添加摄像头

几秒钟后，这将生成一个名为 `annotated.jpg` 的文件，其中包含我们的图像，并在顶部角落用红色写着 **Camera 1**。当我们将所有这些内容整合到最终系统中时，我们还将把图像叠加时间戳。

![](img/631e8b200cce187082ee46b0eb1681b3_380_0.png)

> 目前，`raspistill` 工具生成的图像相当大，因为它们是高分辨率照片。这使得处理和发送它们在处理时间方面有点耗时，因此当我们构建最终系统时，我们将使用 `raspistill` 选项 `-w`、`-h` 和 `-q` 来减小图像的大小和质量，使系统更高效。

要捕获更小的图像文件，请尝试使用以下命令：

```bash
$ raspistill -o test.img -h 768 -w 1024 -q 25
```

## 夜视功能

标准 Raspberry Pi 摄像头非常适合在白天拍摄人们走上花园小径的快照，但在夜间拍摄时，它并不太合适。解决这个问题有两种方法：第一种是在 PIR 探测器被触发时用明亮的灯光照亮捕获区域，第二种是使用 Raspberry Pi **NoIR 摄像头模块**和红外 LED 阵列，让摄像头在黑暗中看到。稍后会详细介绍。

