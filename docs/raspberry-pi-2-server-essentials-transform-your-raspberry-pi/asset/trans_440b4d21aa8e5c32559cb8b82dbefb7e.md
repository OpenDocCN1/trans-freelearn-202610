
## 8 将 Pi 设置为媒体中心服务器

树莓派具有 HDMI 输出，能够流式传输高清音视频，并且还支持 CEC 以共享遥控功能。在本章中，我们将介绍如何直接从命令行使用树莓派作为媒体服务器，显示图像和播放音频。我们将简要介绍人们提出的某些其他解决方案，最后安装 OSMC（以前称为 RaspBMC——一个 XBMC 媒体中心），它开箱即用硬件解码和 CEC。

如果你要使用 Raspbian 进行这些示例，建议分配更多 GPU 内存。树莓派 2 推荐的值为 512MB。你可以通过在命令行中输入 `raspi-config` 来更改核心系统设置。

### 幻灯片

Linux 用户熟悉名为 `fbi`（帧缓冲图像查看器）的命令行程序。我们将使用 HDMI 电缆连接宽屏显示器或 HD 电视。

> 您应注意，小于 19 英寸的方形比例计算机显示器或非 HD 电视通常不被 HDMI 输出支持。

有一个名为 HDMI Pi 的项目，提供了一款经济实惠的 9 英寸、HD 1280 x 800 LCD 屏幕，可直接插入 HDMI 端口而无需任何额外部件。还有其他更小的 LCD 屏幕可供选择，但它们可能相当昂贵，并且需要检查兼容性。

### 使用 fbi

在树莓派上，您可以加载来自高质量相机的原始尺寸照片，但建议使用高速 Class 10 SD 卡或其他快速存储。新的四核处理器可显著加快加载时间。在树莓派 1 上，建议缩小照片尺寸以帮助加载时间。对于树莓派，仍然建议缩小尺寸，但您可以轻松将图像保持在 HD 分辨率，约为 300 万像素（2048 x 1536）。为了获得最清晰的图像，建议将图像分辨率与显示分辨率和宽高比相匹配。

通过 SSH 使用以下命令行。这将安装 fbi 并通过 SSH 启动简单的幻灯片放映，但输出将是 HDMI 上连接的显示器：

```
sudo apt-get install fbi
cd /home/pi/photos
sudo fbi -T 1 -a -noverbose -t 5 *.*
```

fbi 现在将循环浏览目录中的所有图像，每 5 秒更换一次，直到您停止它。

-T 1 命令告诉 fbi 使用终端 1 输出（仅在 SSH 中），这将是 HDMI 端口。我们告诉它自动调整图像大小以适应屏幕，而 -a -t 5 每 5 秒更换一次图像，-noverbose 禁用任何调试信息。

您会注意到 fbi 不会阻塞控制台并在后台运行。可以再次运行 fbi，它不会抱怨另一个实例。然而，这可能会导致内存不足错误和不想要的行为。

要停止 fbi，我们使用 killall 命令：

```
sudo killall fbi
```

### 播放视频

我们可以使用 OMXPlayer 播放视频和音频文件，它是专门由 XBMC 项目设计的，并在树莓派上使用硬件解码。由于 OMXPlayer 使用硬件解码器，您可以使用任何额外购买的视频许可证。

> OMXPlayer 运行至少需要 256 MB GPU 内存。

### OMXPlayer for video playback

将 MP4 文件下载到树莓派上（例如，我们可以使用网站 http://www.hd-trailers.net/ 找到一些链接），我们将使用 OMXPlayer 将视频和声音播放到 HDMI 端口。您也可以直接使用 RTMP 流代替视频文件。将 wget 中的 URL 替换为互联网上现有的文件：

```
sudo apt-get install omxplayer
cd /tmp
sudo wget http://videosite.com/filename.mp4
omxplayer -o hdmi filename.mp4
```

### 播放音频

最新 Raspbian 镜像已安装所有声音驱动程序和实用工具。使用的软件包属于 ALSA。树莓派无法录制音频，因为它没有麦克风插孔，并且 GPIO 引脚都是数字的。要使用 GPIO 录制音频，我们需要使用 A/D（模拟-数字）设备或具有麦克风输入的 USB 声音设备。

### Aplay for audio playback

以下是一个预安装的软件包，可播放 WAV 文件：

```
cd /tmp
wget http://goo.gl/Ps3paV
mv Ps3paV siren.wav
aplay siren.wav
```

### OMXPlayer for audio playback

OMXPlayer 不仅用于播放视频。它还支持音频文件的播放，例如 MP3 文件，如果可能的话会尝试使用硬件解码：

```
omxplayer audio-test.mp3
```

### 使用 AirPlayer

有一个名为 shairport 的项目在树莓派上运行得非常好。它不支持视频或照片，但其中的音乐流媒体非常稳定。我们需要获取该项目并编译它。这只需要几分钟：

```
cd /tmp
git clone -b 1.0-dev git://github.com/abrasive/shairport.git
cd shairport
sudo ./configure && sudo make && sudo make install
```

/tmp 上的文件将在下次启动时被删除，这没问题，因为二进制文件现在已安装在树莓派上。使用以下命令运行服务器，AirPlay 图标将显示在播放音乐的 Apple 设备上：

```
shairport -a 'ShairPortPi'
```

这将阻塞控制台，您会不时看到有关数据包的一些消息弹出，但这是正常的。

### 使用 alsamixer

您可以登录到另一个 SSH 控制台并使用 alsamixer 控制音量。树莓派默认只有一个输出，按上或下键会使声音更大或更小：

```
alsamixer
```

### 安装 OSMC

OSMC 是一个免费开源的媒体中心，最初称为 RaspBMC；它基于 XBMC。在本节中，我们将在 SD 卡上烧录新镜像。您可以使用另一张 SD 卡或擦除到目前为止一直使用的卡。

我们可以使用适用于 Windows、Mac OS X 和 Linux 的 UI 安装程序：

- 1. 打开浏览器并导航到 https://osmc.tv/download.
- 2. 选择与您将从中烧录镜像的操作系统相匹配的 UI 下载链接；下载并运行它。
- 3. 选择您的语言，然后选择 Raspberry Pi 2.
- 4. 在以下屏幕中，尝试选择最新版本。

> 网络镜像意味着在首次启动时，它将下载最新文件，因此建议在首次启动时连接有互联网访问的有线网络。

UI 安装程序还为我们提供了一个选项，可将 OSMC 安装在 USB 驱动器而不是 SD 卡上。如果您选择此选项，您需要在首次启动时连接 USB 存储设备。您还需要一个键盘。

首次启动可能需要大约 10 分钟才能完成，之后它将重新启动，安装即完成。

### 配置 OSMC

OSMC 可以从用户界面进行配置，但您仍然可以登录 SSH 来安装自己的服务或调整一些高级设置。默认的 root 登录名和密码是 OSMC，如果您将浏览器导航到 http://osmc.local，您还将获得一个网络遥控器。

> 在遥控器页面上，您可以使用键盘箭头来控制用户界面，而无需点击按钮。

### 启用其他编解码器

OSMC 使用 OMXPlayer，它由 XBMC 维护。正如我们所知，OMXPlayer 使用 Pi 的硬件来解码视频并渲染 GUI。

如果您有 MPEG-2 或 VC-1 编码的文件，您只需购买许可证并使用 OSMC 界面输入代码即可。H264 编解码器已包含在树莓派的价格许可中。通过导航到：Programs | OSMC Settings | Pi Config (Raspberry ICON) | GPU Mem & Config 然后输入您的密钥并重新启动系统。

### 无线配置

如果您已插入无线适配器，您应该能够在首次设置向导期间设置无线连接。您仍然可以配置 Wi-Fi，并且如果您的适配器支持，您还可以启用网络共享（称为接入点），这是一个很棒的功能，如果您的树莓派通过以太网连接，可以扩展您的 Wi-Fi 范围。导航到 Programs | OSMC Settings | Network (3 CONNECTED CIRCLES ICON)
