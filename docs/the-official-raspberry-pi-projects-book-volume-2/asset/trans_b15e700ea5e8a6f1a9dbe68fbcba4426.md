
为了让这些手势始终被检测到，请花一些时间练习。你可以编辑 test.py 脚本并注释掉 @skywriter.move() 函数（它会显示 x/y/z 数字），以便更容易看到你是否掌握了其中一个手势。

## >STEP-04
### 配置你的演示软件

现在是准备你的演示软件的时候了。LibreOffice Impress 与 Microsoft PowerPoint 非常相似，在 Raspberry Pi 上运行良好。如果不需要 LibreOffice 套件中的其他应用程序，你只需使用以下命令安装 Impress 和核心组件：

```
sudo apt-get install libreoffice-impress
```

你将使用 Python 代码通过 Skywriter 检测手势并生成键盘敲击来以正常方式控制 Impress。唯一额外的配置是激活允许你用鼠标在幻灯片上绘图的功能。这可以通过选择 Slide Show>Slide Show Settings 并勾选 ‘Mouse pointer as pen’ 复选框来完成。

## >STEP-05
### 使用代码

键入右侧列表中的代码并将其保存为 magic_control.py。你可以使用 IDLE 或你选择的任何其他文本编辑器来完成此操作。启动 LibreOffice Impress 并打开你的演示文稿。然后最小化 Impress 并通过 Idle 或通过命令行运行你的代码：

```
sudo python magic_control.py
```

现在切换回 Impress 并准备开始你的演讲或演示。

## >STEP-06
### 惊艳你的观众

通过在 Skywriter 上向上滑动开始幻灯片放映。你可以通过左右滑动（向前）或右向左滑动（向后）在幻灯片之间导航。要结束演示，请在 Skywriter 上向下滑动。

要注释幻灯片，你需要激活绘图功能。使用 tap/touch 来完成此操作：现在你将用手部动作控制鼠标，在屏幕上留下痕迹，就像用笔绘画一样。这需要练习，所以请确保你提前花了一些时间准备！绘图完成后，再次 tap/touch 以解除笔功能。

我们都知道，在现场表演的压力下，事情可能会出错。如果你想快速禁用 Skywriter 对演示的控制，请使用双击退出 Python 程序。

## Magic_Control.py

语言
>PYTHON
下载:
magpi.cc/
2dXnBko

```python
#!/usr/bin/env python
import skywriter
import signal
import autopy
import sys

mouse_down = False
#work out how big the screen we're using is.
width, height = autopy.screen.get_size()

@skywriter.move()
def move(x, y, z):
    #print( x, y, z )
    global mouse_down
    if mouse_down: # Only run if we're in drawing mode
        x = (x) * width
        y = (y) * height
        # scale to screen size
        x = int(x)
        y = height - int(y)

        if( y > 799 ):
            y = 799

        autopy.mouse.move(x, y)
        #print( int(x), int(y) )

@skywriter.flick()
def flick(start, finish):
    print('Got a flick!', start, finish)
    if start == "east":  # Back through Impress slides
        autopy.key.tap(autopy.key.K_LEFT)
    if start == "west":  # Forward through Impress slides
        autopy.key.tap(autopy.key.K_RIGHT)
    if start == "north": # Start slideshow
        autopy.key.tap(autopy.key.K_F5)
    if start == "south": # Quit slideshow
        autopy.key.tap(autopy.key.K_ESCAPE)

@skywriter.double_tap()
def doubletap(position):
    print('Double tap!', position)
    sys.exit()  # Emergency stop

@skywriter.tap()
def tap(position):
    global mouse_down
    print('Tap!', position)
    if mouse_down:  # Toggle mouse up/down
        autopy.mouse.toggle(False)
        mouse_down = False
    else:
        autopy.mouse.toggle(True)
        mouse_down = True

#@skywriter.touch()
#def touch(position):
#  print('Touch!', position)

signal.pause()
```

## 自制你的 Pi HAT

有一些很棒的 HAT 可用于增强你的 Pi 的硬件功能。是否曾想过如何设计和构建一个原型 HAT？本教程将向你展示如何入门

### 你需要

- 一个 EEPROM（推荐使用 CAT24C32）
  magpi.cc/1RwAaQP
- 一个面包板和一些跳线
  magpi.cc/1RwAdMt
- 一些 LED
  magpi.cc/1RwAiQi
- Pi HAT 规范仓库
  magpi.cc/10AHAgK

RICHARD HAYLER
Richard 是 CoderDojo Ham 的导师，他的学校 CodeClub 是 Primary Astro Pi 竞赛的获胜队伍之一，他们的代码现在在国际空间站上。
magpi.cc/41Lmeoi
@rdhayler

![](img/d8322379620245fcd21eccc3855b8c78_121_0.png)

一旦准备就绪，就将所有组件焊接到原型 PCB 板上

我们的简单 HAT 只有一个组件——一个 LED

为了符合正式的 HAT 规范，板上必须有一个 EEPROM

HAT 最早于 2014 年 Raspberry Pi Model B+ 发布时出现。从那时起，出现了一些令人惊叹和美妙的 HAT，其中许多已在《The MagPi》的往期中出现：Unicorn HAT、Piano HAT、Sense HAT 和 Explorer HAT。官方上，HAT 代表 ‘Hardware Attached on Top,’ 尽管你的专家怀疑这个缩写是在名字之后才出现的。你还能怎样称呼一块放在 Pi 顶部的扩展板呢？虽然你可以制造和销售不符合 HAT 标准的板子，但这样做有很多优点。具有相同的物理尺寸使得确保你的板子能够

与大多数 Pi 外壳兼容，并且包含 EEPROM 允许操作系统在启动时识别 HAT 并设置任何所需的硬件。这些说明适用于最新版本的 Raspbian（Jessie）或旧的 Wheezy。如往常一样，开始前运行 sudo apt-get update 和 sudo apt-get upgrade。然后使用以下命令启用内核对 I²C 的支持：

```
sudo raspi-config
```

选择 ‘Advanced Options’，然后向下点按并选择 ‘A7 I2C’。确保两个问题的答案都高亮显示为 ‘Yes’。或者，如果你使用的是 Jessie，也可以通过 GUI 中的 Raspberry Pi 配置工具进行设置。

然后系统会提示你重新启动。Pi 重启后，使用以下命令安装 I²C 工具包：

```
sudo apt-get install i2c-tools
```

现在是时候在面包板上构建你的 EEPROM 电路了。按照图 1 所示组装电路。如果你没有使用 24C32，可能需要连接到 SDA 和 SCL 的不同引脚。检查你的 IC 的数据表以确保正确连接。你还需要为 HAT 提供一些功能：为了保持简单，图 1 的电路只包含一个三色 RGB LED，但你可以根据想要 HAT 实现的功能使用任何你想要的组件。

为 Pi 上电。你可能会注意到 LED 已经亮了（也许只是微弱地）。这是因为 Pi 不知道连接了什么，因此 GPIO 输出处于 ‘floating’ 状态。我们的 HAT 将通过预配置它使用的引脚来处理这个问题。下一页的代码片段（myoh-rgb.py）提供了与 LED 交互的简单 Python 代码；你需要安装出色的 gpiozero 库才能使用它。

接下来，检查第二个 I²C 总线上检测到了哪些 I²C 设备：

```
i2cdetect -y 1
```

![](img/d8322379620245fcd21eccc3855b8c78_122_0.png)

### 什么是 EEPROM？

EEPROM 代表 'Electrically Erasable Programmable Read-Only Memory' 并且是一种用于电子设备中存储少量数据的芯片。

左侧 先在面包板或 Explorer HAT 上构建电路以确保其工作正常，以便你可以进行更改

图 1 EEPROM 应连接到 SDA 和 SCL 以进行编程。如果你没有使用 24C32，可能需要连接到不同的引脚（检查你的 IC 的数据表）

![](img/d8322379620245fcd21eccc3855b8c78_122_1.png)

图 2 为了测试你的 HAT，请确保重新连接 EEPROM，使其连接到 ID_SC 和 ID_SD。你还需要添加两个上拉电阻

![](img/d8322379620245fcd21eccc3855b8c78_122_2.png)

