
作为一个练习，尝试将 `normalize` 设为 `False`，为未归一化的盒式滤波器调用 `cv2.boxFilter()`，并查看输出。

`cv2.GaussianBlur()` 函数使用高斯核代替要应用的盒式滤波器。该滤波器对高斯噪声非常有效。以下是可以用于此函数的代码：

```
output = cv2.GaussianBlur(input, (3,3),0)
```

> 你可能想阅读更多关于高斯噪声的内容，网址为 http://homepages.inf.ed.ac.uk/rbf/HIPR2/noise.htm。

以下是前面代码的输出，其中输入是带有高斯噪声的图像，输出是去除了高斯噪声的图像。

![](img/631e8b200cce187082ee46b0eb1681b3_157_0.png)

## 创建你自己的运动检测与跟踪系统

`cv2.medianBlur()` 用于使用中值滤波器对图像进行中值模糊处理。它计算内核下所有值的中值，并将内核中的中心像素替换为计算出的中值。在此滤波器中，窗口沿图像滑动，窗口内像素的中值强度值成为被处理像素的输出强度。这对椒盐噪声非常有效。我们需要向该函数传递一个输入图像和一个奇数正整数（而不是前两个函数那样的行列元组）。以下代码在图像中引入椒盐噪声，然后应用 `cv2.medianBlur()` 来去除噪声：

```
```python
import cv2
import numpy as np
import random
from matplotlib
import pyplot as plt

img = cv2.imread('lena_color_512.tif', 1)

input = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

output = np.zeros(input.shape, np.uint8)
p = 0.2# probability of noise
for i in range(input.shape[0]):
    for j in range(input.shape[1]):
        r = random.random()
        if r < p / 2:
            output[i][j] = 0, 0, 0
        elif r < p:
            output[i][j] = 255, 255, 255
        else:
            output[i][j] = input[i][j]

noise_removed = cv2.medianBlur(output, 3)

plt.subplot(121), plt.imshow(output), plt.title('Noisy Image')
plt.xticks([]), plt.yticks([])
plt.subplot(122), plt.imshow(noise_removed), plt.title('Median Filtering')
plt.xticks([]), plt.yticks([])
plt.show()
```
```

你会发现椒盐噪声被大幅减少，图像对人眼来说也更加易于理解。

```
erosion = cv2.erode(img,kernel,iterations = 2)
dilation = cv2.dilate(img,kernel,iterations = 2)
gradient = cv2.morphologyEx(img, cv2.MORPH_GRADIENT, kernel)

titles=['Original','Erosion','Dilation','Gradient']
output=[img,erosion,dilation,gradient]

for i in xrange(4):
    plt.subplot(2,2,i+1),plt.imshow(output[i],cmap='gray')
    plt.title(titles[i]),plt.xticks([]),plt.yticks([])
plt.show()
```

输出将如下所示：

![](img/631e8b200cce187082ee46b0eb1681b3_160_0.png)

## 运动检测与跟踪

现在，我们将使用简单的逻辑构建一个成熟的运动检测与跟踪系统：找出视频源（如网络摄像头流）中后续帧之间的差异，并在检测到差异的区域周围绘制轮廓。

让我们导入所需的库并初始化网络摄像头：

```
import cv2
import numpy as np

cap = cv2.VideoCapture(0)
```

我们需要一个用于膨胀操作的内核，我们将提前创建它，而不是在循环中每次都创建：

```
k=np.ones((3,3),np.uint8)
```

以下代码将捕获并存储后续帧：

```
t0 = cap.read()[1]
t1 = cap.read()[1]
```

现在我们启动 while 循环，计算帧之间的差异，并将输出转换为灰度图像以供进一步处理：

```
while(True):
	d=cv2.absdiff(t1,t0)

grey = cv2.cvtColor(d, cv2.COLOR_BGR2GRAY)
```

输出将如下所示，它显示了帧之间像素的差异：

![](img/631e8b200cce187082ee46b0eb1681b3_161_0.png)

此图像可能包含一些噪声，因此我们将首先对其进行模糊处理：

```
blur = cv2.GaussianBlur(grey,(3,3),0)
```

我们使用二进制阈值将这种去噪后的输出转换为二值图像，代码如下：

```
ret, th = cv2.threshold( blur, 15, 255, cv2.THRESH_BINARY )
```

最后的操作是膨胀图像，以便我们更容易清晰地找到边界：

```
dilated=cv2.dilate(th,k,iterations=2)
```

上一步的输出如下：

![](img/631e8b200cce187082ee46b0eb1681b3_162_0.png)

然后，我们用以下代码为前面的图像查找并绘制轮廓：

```
contours, hierarchy =
cv2.findContours(dilated,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)

t2=t0
cv2.drawContours(t2, contours, -1, (0,255,0), 2 )

cv2.imshow('Output', t2 )
```

最后，我们将最新帧赋给旧帧，并用以下代码通过网络摄像头捕获下一帧：

```
t0=t1
t1=cap.read()[1]
```

像往常一样，一旦检测到 Esc 键按下，我们就终止循环：

```
if cv2.waitKey(5) == 27 :
    break
```

循环终止后，我们释放摄像头并销毁显示窗口：

```
cap.release()
cv2.destroyAllWindows()
```

这将在检测到运动的区域大致绘制轮廓，如下面的截图所示：

![](img/631e8b200cce187082ee46b0eb1681b3_163_0.png)

这段代码对于缓慢移动效果很好。你可以通过使用不同颜色绘制轮廓来使输出更有趣。此外，你可以找出轮廓的质心，并绘制与质心对应的十字线或圆。

> 如果你想更深入地探索 OpenCV 与 Raspberry Pi 的结合使用，请阅读《Raspberry Pi Computer Vision Programming》。链接如下：https://www.packtpub.com/hardware-and-creative/raspberry-pi-computer-vision-programming。

## 小结

在本章中，我们学习了使用 OpenCV 和 Pi 进行计算机视觉的高级主题。

我们学习了高级图像处理技术，如阈值处理、降噪、轮廓和形态学操作。最后，我们实现了所有这些技术，构建了一个真实的图像处理应用。

在下一章中，我们将学习将 Pi 与 Grove Shield 和 Grove 传感器接口连接的基础知识。

你可以使用传感器与周围世界交互并进行监控，然后将数据存储在 Raspberry Pi 上，从而弥合与现实世界的鸿沟！

![](img/631e8b200cce187082ee46b0eb1681b3_166_0.png)

从前面的图片可以看出，GrovePi 直接插在 Raspberry Pi 上，具有多种插座，可容纳你可能想要添加到 Raspberry Pi 上的不同传感器。它与 Raspberry Pi 型号 A、A+、B、B+、2 兼容，并使用其创建者提供的开源库，允许用户使用 Python、C、C++、Go 和 NodeJS 进行编程。

在本章中，我们将需要以下组件：

- 湿度和温度传感器 (http://www.seeedstudio.com/depot/Grove-TempHumi-Sensor-p-745.html?cPath=25_125)
- LED (http://www.seeedstudio.com/depot/Grove-Red-LED-p-1142.html?cPath=81_35)
- 超声波测距仪 (http://www.seeedstudio.com/depot/Grove-Ultrasonic-Ranger-p-960.html?cPath=25_31)
- OLED 显示屏 (http://www.seeedstudio.com/depot/Grove-OLED-Display-096-p-824.html?cPath=34_36)
- 蜂鸣器 (http://www.seeedstudio.com/depot/Grove-Buzzer-p-768.html?cPath=38)
- GrovePi+ (http://www.seeedstudio.com/depot/GrovePi-p-2241.html)

以上所有组件都可以在 SeeedStudio 网站上找到。

> 你可以在其创建者的网站上了解更多关于 GrovePi 和传感器的信息：
> http://www.dexterindustries.com/

## 设置 GrovePi

让我们看一下 GrovePi 的图片：

![](img/631e8b200cce187082ee46b0eb1681b3_167_0.png)

从前面的图片可以看出，GrovePi 有一个插座，可以直接插入 Raspberry Pi。完成此操作后，我们就可以在 Raspberry Pi 上设置软件来使用该扩展板。步骤如下。

- 1. 在不连接 GrovePi 的情况下打开 Raspberry Pi，通过显示器或 ssh 打开终端窗口：

![](img/631e8b200cce187082ee46b0eb1681b3_168_0.png)

- 2. 建议将 GrovePi 安装在桌面上，因此我们首先使用以下命令进入桌面目录：
- $ cd /home/pi/Desktop

- 3. 现在使用以下命令将 GitHub 仓库克隆到桌面：
- $ sudo git clone https://github.com/DexterInd/GrovePi

![](img/631e8b200cce187082ee46b0eb1681b3_168_1.png)

