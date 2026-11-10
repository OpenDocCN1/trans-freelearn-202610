
## 教程

## 逐步操作

## 日常工程

## 第11部分

## SIMON MONK

Simon Monk 是《Raspberry Pi Cookbook》和《Programming the Raspberry Pi: Getting Started with Python》等书籍的作者。
simonmonk.org
monkmakes.com

## PI MI-LIGHT

使用你的 Raspberry Pi 以及知名技术黑客和作者 Simon Monk 的帮助，解决现实世界的电子和工程问题

## 所需材料

- NRF24L01
  2.4GHz 无线射频收发模块（eBay）
- Mi-Light RGB LED 灯泡
- Mi-Light 2.4GHz 遥控器
- 7 × 母对母跳线

Mi-Light 灯泡看起来就像普通的 LED 灯泡，价格也相近，但它们内置了 2.4GHz RF 无线电链接。 这可以与 RF 遥控器配合使用来开关灯光，灯光分为四个区域分组。 遥控器还可以调节灯光的亮度和颜色。 Mi-Light 生产了一种现成的模块，可以将灯具连接到 WiFi 路由器，然后使用智能手机应用程序控制照明。 然而，通过使用连接到 Raspberry Pi 的 £3（$5）无线电模块，你可以让 Raspberry Pi 控制家中的 Mi-Light 灯泡，从而为家庭自动化打开各种可能性。

一个可能的用途是让 Raspberry Pi 作为 Web 服务器运行，为你提供一个 Web 界面，让你可以通过连接到网络的任何设备的浏览器打开和关闭灯光。

![](img/d8322379620245fcd21eccc3855b8c78_133_0.png)

Mi-Light LED 灯泡

NRF24L01 无线电收发器

本项目基于 Torsten Tränkner 的一篇原创博客文章，你可以在这里用德语找到：torsten-traenkner.de。 该项目仅在 Raspberry Pi 2 上测试过，可能需要对其他型号的 Pi 进行一些调整。

正如所需组件列表所示，本项目不需要任何焊接。 RF 模块使用母对母跳线连接到 Raspberry Pi。 即使最终 Raspberry Pi 将接管灯光的操作，你在最初设置时仍需要一个 Mi-Light 遥控器。

![](img/d8322379620245fcd21eccc3855b8c78_134_0.png)

## 搭建你的 PI-MI-LIGHT 控制器

![](img/d8322379620245fcd21eccc3855b8c78_134_1.png)

![](img/d8322379620245fcd21eccc3855b8c78_134_2.png)

你应该在 Raspberry Pi 关闭电源的情况下接线 RF 模块，并在重新通电前非常仔细地检查所有连接是否正确。 使用像 Raspberry Leaf（由 MonkMakes 提供）这样的 GPIO 模板将有助于你在 GPIO 连接器上找到正确的连接。

![](img/d8322379620245fcd21eccc3855b8c78_134_3.png)

### 步骤-01
将引线连接到 RF 模块

首先使用接线图作为参考将跳线连接到 RF 模块。 你需要连接到每个引脚，除了连接器右下角的引脚（引脚 8）。 使用与接线图相同颜色的引线将有助于你做出正确的连接。

### 步骤-02
连接 RF 模块

按如下方式将 RF 模块的引线连接到 GPIO 接头：
- RF 模块的引脚 1（GND）连接到 GPIO 接头的 GND。
- RF 模块的引脚 2（VCC）连接到 GPIO 接头的 3.3V。
- RF 模块的引脚 3（CE）连接到 GPIO 25。
- RF 模块的引脚 4（CSN）连接到 GPIO 8。
- RF 模块的引脚 5（CLK）连接到 GPIO 接头的 SCLK（GPIO 11）。
- RF 模块的引脚 6（MOSI）连接到 GPIO 接头的 MOSI（GPIO 10）。
- RF 模块的引脚 7（MISO）连接到 GPIO 接头的 MISO（GPIO 9）。

![](img/d8322379620245fcd21eccc3855b8c78_134_4.png)

### 步骤-03
配对 Mi-Light

对于此步骤，你需要将灯泡插入合适的灯座。 然后打开正常的灯开关并立即按下 Mi-Light 遥控器上四个灯区之一的开按钮。 灯泡会闪烁三次作为确认。 配对后，遥控器将能够打开和关闭灯光并进行其他调整。

在你开始使用它之前，本项目需要在软件设置上花费相当多的时间。
无线电模块使用 Raspberry Pi 的 SPI 接口，需要启用它，因此使用以下命令运行 raspi-config：

```
sudo raspi-config
```

向下滚动到高级选项，选择它，然后选择 SPI。 启用它，并在询问是否

> 这段代码全部用 C++ 编写，但可以从我们的 Python 程序中调用

是否自动加载 SPI 模块时，选择是。
接下来，你需要通过发出以下命令为 RF 模块安装并构建 NRF24 库：

```
git clone https://github.com/TMRh20/RF24
cd RF24
make all
sudo make install
```

之后，你需要使用以下命令下载一个消息库，该库为基本 RF 模块提供更高级别的接口：

```
cd ..
git clone https://github.com/mysensors/Raspberry
cd Raspberry
make
sudo make install
```

下面 从手机浏览器打开/关闭灯区

现在使用以下命令下载 Torsten Tränkner 编写的代码：

![](img/d8322379620245fcd21eccc3855b8c78_135_0.png)

```
cd ..
wget http://torsten-traenkner.de/wissen/smart_home/openmilight_raspberry_pi.tgz
tar xzvf openmilight_raspberry_pi.tgz
```

这段代码全部用 C++ 编写，因此为了在 Python 中使用它，你的专家编写了一个名为 send_cmd 的 C++ 程序，该程序旨在从 Python 调用并发送要发送到 Mi-Light 的命令消息。 使用以下命令从你的 Pi 命令行下载此程序和项目的 Python 代码：

```
git clone https://github.com/simonmonk/pi_magazine.git
```

将项目文件夹 11_mi_light 中的所有文件复制到 openmilight 文件夹中，然后使用以下命令编译其中两个程序：

```
cp pi_magazine/11_mi_light/* openmilight
cd openmilight
sh ./comp.sh
```

你还需要使用以下命令安装将用于 Web 服务器的 Python Bottle 库：

```
sudo apt-get install python-bottle
```

在你运行主程序之前，你需要找到用于与 Mi-Light 配对的 Mi-Light 遥控器的 ID，以便你的 Raspberry Pi 可以冒充它。
使用以下命令运行 openmilight 程序，然后在遥控器上按几次区域 1 的开按钮。 每次按下按钮时，你应该会看到如下示例中的十六进制数字流。

```
sudo ./openmilight
in listening mode
B0 A1 56 41 C1 03 72 .
B0 A1 56 41 C1 03 73 .
```

前三个数字（在本例中为 B0、A1 和 56）对于你的遥控器会有所不同，因此使用 CTRL+C 退出程序并编辑文件 lights.py，将文件顶部 ID 变量的值更改为你的遥控器的三个十六进制数字。
最后，你可以使用以下命令启动 Web 服务器：

```
sudo python lights.py
```

将方便的 Web 浏览器指向你的 Raspberry Pi 的 IP 地址，你将看到一个网页，其中包含所有四个区域的开和关按钮。 单击区域 1 的按钮，你应该能够打开和关闭连接到该区域的所有灯光。

## Lights.py

```
from bottle import route, run, template, request
import os

ID = 'B0 A1 56'
MIDDLE = '06 C9'

ZONES = {
  'zone1': {'on': '03', 'off': '04'},
  'zone2': {'on': '05', 'off': '06'},
  'zone3': {'on': '07', 'off': '08'},
  'zone4': {'on': '09', 'off': '0A'}
}

def light_on(zone):
  print(zone + "on")
  send(ZONES[zone]['on'])

def light_off(zone):
  print(zone + "off")
  send(ZONES[zone]['off'])

def send(code):
  global ID, MIDDLE
  for i in range(1, 5):
    message = ID + MIDDLE + code + '00'
    os.system('./send_cmd "' + message + '"')

# Handler for the home page
@route('/')
def index():
  zone = request.GET.get('zone', 'zone1')
  state = request.GET.get('state', 'off')
  if state == 'on':
    light_on(zone)
  else:
    light_off(zone)
  return template('home.tpl')

# Start the webserver running on port 80
run(host="0.0.0.0", port=80)
```

## 代码如何工作

在我们讲解时，将代码放在编辑器中会很有帮助。

程序首先导入 bottle 和 os 库。 os 库是必需的，以便从 Python 调用 send_cmd C 程序。

常量 ID 需要设置为你的遥控器的三个十六进制数字，如前所述。 MIDDLE 中的代码不需要更改。 请注意，这两个常量中的空格必须保留原样，以便在构建十六进制消息时，每个十六进制数字之间都有空格。

Mi-Light 遥控器允许定义多达四个区域，你可以为每个区域连接多个灯。 有单独的开和关代码可以独立控制每个区域。 这些存储在变量 ZONES 中，它是一个字典的字典，使得查找你想要的命令的适当十六进制代码变得很容易。

两个函数（light_on 和 light_off）根据作为其参数指定的区域名称打开或关闭所有灯光。 这两个函数都使用 send 函数通过 C 程序（send_cmd）实际将命令发送到 RF 模块。 send 函数首先通过连接 ID、MIDDLE 部分、命令代码，最后是 00 构造一个消息字符串。 然后调用 C 程序五次，将消息作为参数传递。 由于它可能有点不可靠，因此发送五次消息几乎可以确保将其发送到灯泡。

接下来是代码的 Web 服务器部分，包含在 index 函数中。 它期望网页提供两个请求参数：区域名称（zone）和是否打开或关闭（state）。 然后使用此信息调用 light_on 或 light_off。 在 index 函数的末尾，返回模板 home.tpl 的内容，以向浏览器提供 Web 界面的 HTML。

代码的最后一行启动 Web 服务器。 切换到 Raspberry Pi 上的浏览器，或网络上的另一台计算机，并在地址栏中输入你的 Pi 的 IP 地址。 要发现你的 Raspberry Pi 的 IP 地址，请在 LXTerminal 中输入以下命令：

hostname -I

它将在响应的开头作为四个用点分隔的数字；例如，192.168.1.22。

## 使用你的 PiMi-Light 控制器

除了从计算机上的浏览器控制灯光外，你也可以同样轻松地使用连接到与 Raspberry Pi 相同网络的智能手机上的浏览器。

你还可以改编《The MagPi》第 33 期中的闹钟程序，以在特定时间打开和关闭灯光。

本项目仅打开和关闭照明。 已经做了大量工作来逆向工程 Mi-Light 协议，以弄清楚所有代码的作用。 因此，如果你想扩展此项目以调整灯光的颜色和亮度，那么你可能想查看 Henryk Płotz 的工作：magpi.cc/1NZfcmt
