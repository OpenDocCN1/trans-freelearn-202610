

## Maingame-Uni.py

```
import unicornhat as uh #import the necessary modules
import time, random,signal
import catchphrases_uni as cp # the file with our animations
import RPi.GPIO as GPIO
from fuzzywuzzy import fuzz

def halfthing(num,colour): #turns on half the LEDS to indicate who pressed button first
    uh.clear()
    for x in range (num,num+4):
        for y in range(0,8):
            uh.set_pixel(x,y,colour[0],colour[1],colour[2])
    uh.show()

def pressed(num): # Function to be run when buttons pressed
    global running
    running = False # stops animation from playing
    halfthing(num,(255,0,0))
```

```
get_answer(3) # call with goes set to 3

def b21pressed(channel): # Run when button on GPIO 21 pressed
    pressed(0)

def b16pressed(channel): # Run when button on GPIO 16 pressed
    pressed(4)

def get_answer(goes): # Asks the player for their answer
    answer = raw_input('Name that catchphrase? ')
    match = fuzz.ratio(answer, picked) # compare answer to catchphrase title
    if match >=85:
        print 'Correct!'
    elif match >= 60 and match < 85: # if it nearly matches
        goes=goes-1
        if goes==0:
            print('run out of goes')
```

LED 灯和删除帧。当你对动画满意时，点击‘export to py’，它将被保存为 animation8x8.py。

### >STEP-03
调整动画

然后你需要使用命令行重命名文件（为你自己选择的名称）：

```
mv animation8x8.py yourchoice.py
```

![](img/d8322379620245fcd21eccc3855b8c78_117_0.png)

右图 使用 8x8 网格动画程序创建你自己的精彩口号动画

然后你应该重复步骤 02，直到你至少有三个动画。确保在每次导出后重命名它们。

现在我们需要将动画复制到口号代码中。首先，在 IDLE 中打开 yourchoice.py（或你命名的文件），然后找到带有 frames=[ 的行。一直高亮到最后的 ]，然后将其复制并粘贴到名为 catchphrases.py 的新文件中。对其余动画重复相同操作并将它们粘贴到同一文件中。最后，你需要使用 Python 字典和列表将动画与其名称关联起来；一个示例 catchphrases.py 已包含在此项目的 GitHub 仓库中。

### >STEP-04
连接并测试按钮

要将按钮连接到树莓派，你需要三根跳线、一块面包板和两个按钮。要制作电路，请遵循第 55 页的图像；要测试按钮，请使用 listing1.py 代码。如果使用 Sense HAT，请确保使用相同的 GPIO 引脚（16 和 21）。

### >STEP-05
Fuzzywuzzy 匹配

模糊匹配是关于找出一个字符串与另一个字符串有多接近。打开一个 Python shell（使用命令行或 IDLE），然后导入 fuzzywuzzy 库。

```
>>>from fuzzywuzzy import fuzz
```

```
else:
    print 'Close.. Try again (' + ( str(goes) + ' guesses remaining)'
    get_answer(goes)
else:
    print "That's not right"
print 'Press ^C to exit'

def show_catchphrase():  # Display an animation on the LED matrix
    picked = random.choice(cp.ANSWERS.keys()) # Pick random animation from file
    for i in range(3):
        for x in cp.ANSWERS[picked]:
            if running:
                uh.set_pixels(x) # display a frame from animation
                uh.show()
                time.sleep(0.25) #pause before next frame
    time.sleep(1) # pause before we play animation again
```

```
return picked

GPIO.setmode(GPIO.BCM) #set GPIO numbering to BCM
GPIO.setup(21,GPIO.IN, pull_up_down=GPIO.PUD_UP) #Set these pins as inputs
GPIO.setup(16,GPIO.IN, pull_up_down=GPIO.PUD_UP)
#set callback functions that are run when button pressed
GPIO.add_event_detect(21,GPIO.FALLING,callback = b21pressed,bouncetime=300)
GPIO.add_event_detect(16,GPIO.FALLING,callback = b16pressed,bouncetime=300)
try:
    running = True # use this to stop animation when button pressed
    picked = 'blank'
    picked = show_catchphrase()
    signal.pause() #stops the code from exiting
except KeyboardInterrupt:  # catch ^c
    exit() #exit the code
```

Fuzzywuzzy 会为两个短语的接近程度给出 100 分制的分数。

```
>>>fuzz.ratio('hello world','catchphrase')
18
>>>fuzz.ratio('piphra...','pyphase')
80
```

我们可以用它来检查玩家的答案与我们的口号有多接近（因为我们不想太苛刻！）。

### >STEP-06

## 最终代码

现在我们需要把学到的所有内容结合起来创建口号游戏。将主列表（上方）中的代码输入到名为 piphra...py 的文件中，然后使用以下命令运行它：

```
sudo python piphra...py
```

程序应该在等待有人按下任一按钮的同时在 Unicorn HAT 上播放三次动画。当玩家按下其中一个按钮时，他们一侧的 Unicorn HAT 会亮起。然后他们需要输入他们认为正在显示的口号。程序随后会告诉他们答案是否正确或不正确。如果玩家的答案根据模糊匹配在 60 到 80 之间，他们也可能会看到“再试一次”的消息。

![](img/d8322379620245fcd21eccc3855b8c78_118_0.png)

左图 此项目可与 Sense HAT 或 Unicorn HAT 一起使用。如果使用 Unicorn HAT，应像这样连接

## Listing1.py

```
import RPi.GPIO as GPIO #import the necessary modules
import time

GPIO.setmode(GPIO.BCM)  #set GPIO numbering to BCM
GPIO.setup(16,GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(21,GPIO.IN, pull_up_down=GPIO.PUD_UP)

while True:
    print('b1= '+str(GPIO.input(16))+ ' b2= '+str(GPIO.input(21)))
    time.sleep(0.1)
```

## 使用 Skywriter 进行魔术演示

使用 Skywriter HAT 用令人费解的魔术力量或原力的掌控让你的观众惊叹

## RICHARD HAYLER

Richard 是 CoderDojo Ham 的导师。他的学校 CodeClub 是 Primary School Astro Pi 竞赛的获胜团队之一。magpi.cc/1ILmeoi coderdojoham.org

![](img/d8322379620245fcd21eccc3855b8c78_119_0.png)

## 你需要

- Skywriter HAT 或板子商店。pimoroni.com
- Skywriter API 库 magpi.cc/2dXntBn
- Python AutoPy 库 magpi.cc/2dXmbpZ
- LibreOffice Impress libreoffice.org

下方 如果需要，你可以使用更长的线或排线将 Skywriter 板远离 Pi

说实话：仅仅使用树莓派来运行你的演讲幻灯片就会让观众觉得你很酷。但如果这还不够令人印象深刻，为什么不进一步用你的意念移动能力翻阅演示文稿并通过在空中绘画来注释幻灯片呢？你甚至不需要是霍格沃茨或绝地学院的毕业生：只要拥有一台 Skywriter 设备和一些简单的 Python，你就可以开始了。Skywriter 设备使用一排发射电极生成在三维空间中传播的电场。当你将手移动到 Skywriter 上方时，它会扰乱该场，这些变化会被接收电极网格检测到。这些测量值可用于计算你的手的位置和移动方向。

### >STEP-01
连接 Skywriter 设备

如果你有 Skywriter HAT，它就像其他 HAT 一样直接连接到 GPIO 引脚上。如果你有更大的 Skywriter 板，你需要将六个 GPIO 引脚连接到顶部的对应引脚上，如下图所示。

![](img/d8322379620245fcd21eccc3855b8c78_119_1.png)

### >STEP-02
安装软件

确保你拥有最新版本的 Raspbian，并安装了所有更新。像往常一样，那些乐于助人的 Pimoroni 海盗提供了一个单一脚本来处理安装，包括完整的 Python API。像大多数 HAT 一样，Skywriter 需要 Pi 上的 I²C 总线启用，所以如果你尚未在 Pi 上激活它，你需要在 Skywriter 工作前重新启动。

```
curl -sSL get.pimoroni.com/skywriter | bash
```

你还需要 AutoPy Python 库及其依赖项，因此使用以下命令安装这些：

```
sudo apt-get install libx11-dev libxtst-dev
```

...然后：

```
sudo pip install autopy
```

### >STEP-03
测试你的 Skywriter

Python API 提供了示例脚本来帮助你熟悉 Skywriter 的工作原理：

```
cd Pimoroni/skywriter
sudo python test.py
```

现在在 Skywriter 上方轻轻挥动手臂。你应该会看到三列滚动数字，对应于设备上方三轴（x/y/z）盒子中手的位置。Python 库预先配置为识别某些手势：轻扫（在 Skywriter 上滑动）、轻触或触摸（将手降至表面上方一点），以及最棘手的 Airwheel（在 Skywriter 上方以圆形模式摆动手指）。需要一段时间才能掌握
