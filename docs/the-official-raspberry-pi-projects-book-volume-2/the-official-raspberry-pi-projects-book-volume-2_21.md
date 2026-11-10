## PI GLOWBE

## 由 Pi 驱动的唤醒灯

将地球仪变成发光球体，并利用它在黑暗的冬季月份帮助你醒来

冬季，或一年中的任何时间，早上起床都可能很困难。如果你黎明前醒来，你的身体仍然认为还是夜晚（因为确实是）。你可以购买唤醒灯，通过模拟人造日出欺骗身体醒来；它们会在闹钟临近时逐渐变亮。这些灯价格可能相当昂贵，所以为什么不使用 Pi 和一些 LED 自己制作一个呢？为了庆祝国际空间站上的 Astro Pis，我们用慈善商店里找到的地球仪制作了一个，里面装有 Sense HAT——也许是 unwanted Christmas present，现在被重新利用了！

### STEP-01
制作 LED 灯泡

通过将 LED 灯带缠绕在塑料瓶周围来制作灯泡。你可以跳过这一步，但 Sense HAT 不够亮，无法制作灯。剪掉空塑料瓶的顶部，用贴纸垫覆盖它，然后以螺旋方式缠绕 LED 灯带。你可以用胶带进一步固定末端。让连接端位于较宽的开口处，以便将电源线引向 Pi，然后再从中心引出。我们将在倒置的瓶口顶部安装 Pi，所以确保它平整。

### STEP-02
毁灭世界

通过将支架的插脚从北极和南极的孔中拉出，将地球从其支架上取下。接下来，沿赤道将其分成两半，拉开两个半球。它应该很容易拆开，只需在正确的位置施加一点力。你需要制作一个新的支架，防止地球仪滚动，同时仍允许电源线从底部孔中退出——你可以使用胶带卷的内侧。你可以用刀切一个小槽，让电线进入。

### STEP-03
安装在地球仪内部

将新的 LED 灯泡安装在地球仪内部。用 Blu-Tack 或腻子将瓶颈安装在下孔上；它应该足够坚固，可以支撑 Pi 放在其顶部。安装 Sense HAT 到 Pi 上，然后将两者放在倒置的瓶子上。给 Pi 连接一个 USB WiFi 适配器（除非你使用有线以太网），否则它会在错误的时间唤醒你。这是因为 Pi 需要互联网访问来设置时钟。

### STEP-04
布线供电

你可能需要剪断并重新连接电源线以使其穿过孔。官方 Pi 电源使用钟线，这使得用螺丝端子（choc block）重新连接变得容易。不幸的是，它提供的电力不足以在全亮度下使用 LED 并同时运行 Pi。剪断电缆并将供电端穿过孔。在电缆上打一个结，防止它被拉回。分离两端的两个电源芯并剥去绝缘层。用 choc block 重新连接，注意极性。

### STEP-05
下载代码

将代码从 GitHub 仓库克隆到你的主文件夹：

```
git clone https://github.com/jpsingleton/pi-glowbe.git
```

如果你只使用部分硬件或不同的 LED 灯带，可以根据需要调整代码。你可能还需要安装 Sense HAT 软件，如果你运行的是较旧版本的 Raspbian——请按照 magpi.cc/1KboHnN 的说明进行操作。你可能还想禁用 Pi 上的 LED，以免打扰睡眠。较新的型号可以在软件中完成，否则你可以贴上胶带，甚至如果你足够勇敢，可以将其拆焊。

### wakeup.py

语言
>PYTHON 2.7
下载:
magpi.cc/1TUKdz7

```
from BlinkyTape import BlinkyTape
from sense_hat import SenseHat
from time import sleep
import optparse

#### Default Blinky Tape port on Raspberry Pi is /dev/ttyACM0
parser = optparse.OptionParser()
parser.add_option("-p", "--port", dest="portname",
                  help="serial port (ex: /dev/ttyACM0)",
                  default="/dev/ttyACM0")
(options, args) = parser.parse_args()

if options.portname is not None:
    port = options.portname
else:
    print "Usage: python wakeup.py -p <port name>"
    print "(ex.: python wakeup.py -p /dev/ttyACM0)"
    exit()

sense = SenseHat()
bt = BlinkyTape(port)

#### wake phase - gradually get brighter, linearly
sleepTime = 18 # 18 sec for 30 min in 100 steps
maxPower = 100 # flickers or cuts out above 100

for y in xrange(maxPower):
    sense.clear(y * 2, y * 2, y * 2)
    for x in xrange(sleepTime):
        bt.displayColor(y, y, y)
        sleep(1)

#### on phase - at full brightness for the same time
sense.clear(255, 255, 255)
for z in xrange(maxPower * sleepTime):
    bt.displayColor(maxPower, maxPower, maxPower)
    sleep(1)

#### tidy up
sense.clear()
```

### STEP-06
设置你的日程

我们将使用 cron 运行代码。它应该在你起床前约半小时开始，并且可能只在工作日运行。通过在终端中输入以下内容启动 cron 配置：

```
crontab -e
```

接下来，通过在末尾添加以下行来设置它：

```
15 6 * * 1-5 python /home/pi/pi-glowbe/wakeup.py
```

这将在每个工作日 06:15 运行代码，所以根据你的需要更改它。保存文件并退出编辑器（在默认的 nano 中按 CTRL+X，Y，然后 ENTER）。满意后，重新组装所有东西，等待早晨！


