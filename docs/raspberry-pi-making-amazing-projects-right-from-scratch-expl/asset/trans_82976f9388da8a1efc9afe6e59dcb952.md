
## 创建自己的运动检测与跟踪系统

在上一章中，我们学习了OpenCV库的基础知识。我们为树莓派配置了Python环境以进行OpenCV编程，并基于实时网络摄像头输入实现了一个简单的项目，通过颜色跟踪物体。在本章中，我们将学习一些更高级的概念，并基于OpenCV再实现一个项目。在本章中，我们将学习以下主题：

- 阈值化
- 噪声消除
- 图像的形态学操作
- OpenCV中的轮廓
- 实时运动检测与跟踪

## 图像阈值化

**阈值化**是一种图像分割方法。虽然阈值化方法和算法也适用于彩色图像，但它在灰度图像上效果最佳。阈值化通常（但并非总是）将灰度图像转换为二值图像（在二值图像中，每个像素只能取两个可能值之一：白色或黑色）。图像阈值化通常是许多图像处理应用的第一步。

阈值化的工作原理非常简单。我们定义一个阈值。对于灰度图像中的像素，如果灰度强度值大于阈值，则我们为该像素赋一个值（例如白色）；否则，我们为像素赋予黑色值。这是最简单的阈值化形式。此外，这种方法还有许多其他变体，我们现在将逐一了解。

在OpenCV中，`cv2.threshold()` 函数用于对图像进行阈值化。其输入包括灰度图像、阈值、maxVal和阈值方法作为参数，并返回阈值化后的图像作为输出。maxVal是当像素强度大于（在某些方法中为小于）阈值时分配给该像素的值。OpenCV提供了许多阈值方法；最初，我们看到的阈值化最简单形式是 `cv2.THRESH_BINARY`。让我们看看一些阈值方法的数学表示。

假设 (x,y) 是输入像素，那么各阈值方法的操作将如下所示：

- `cv2.THRESH_BINARY`  
  如果 intensity(x,y) > threshold，则设置 intensity(x,y)=maxVal；否则，设置 intensity(x,y)=0
- `cv2.THRESH_BINARY_INV`  
  如果 intensity(x,y) > threshold，则设置 intensity(x,y)=0；否则，设置 intensity(x,y)=maxVal
- `cv2.THRESH_TRUNC`  
  如果 intensity(x,y) > threshold，则设置 intensity(x,y)=threshold；否则，保持 intensity(x,y) 不变
- `cv2.THRESH_TOZERO`  
  如果 intensity(x,y) > threshold，则保持 intensity(x,y) 不变；否则，设置 intensity(x,y)=0
- `cv2.THRESH_TOZERO_INV`  
  如果 intensity(x,y) > threshold，则设置 intensity(x,y)=0；否则，保持 intensity(x,y) 不变

阈值功能的演示通常在具有渐变的灰度图像上效果最佳。在以下示例中，我们将阈值设置为127，因此图像根据像素的强度值被分割成两组像素：

```python
import cv2
import matplotlib.pyplot as plt

img = cv2.imread('gray21.512.tiff',0)
th=127
max_val=255
ret,o1 = cv2.threshold(img,th,max_val,cv2.THRESH_BINARY)
ret,o2 = cv2.threshold(img,th,max_val,cv2.THRESH_BINARY_INV)
ret,o3 = cv2.threshold(img,th,max_val,cv2.THRESH_TOZERO)
ret,o4 = cv2.threshold(img,th,max_val,cv2.THRESH_TOZERO_INV)
ret,o5 = cv2.threshold(img,th,max_val,cv2.THRESH_TRUNC)

titles = ['Input Image','BINARY','BINARY_INV','TOZERO',
          'TOZERO_INV','TRUNC']
output = [img, o1, o2, o3, o4, o5]

for i in xrange(6):
    plt.subplot(2,3,i+1),plt.imshow(output[i],cmap='gray')
    plt.title(titles[i])
    plt.xticks([]),plt.yticks([])
plt.show()
```

上述代码的输出如下：

![](img/631e8b200cce187082ee46b0eb1681b3_151_0.png)

### 大津法

大津阈值化方法会自动确定直方图中有两个峰值的图像（双峰直方图）的阈值。下面是一个双峰直方图：

![](img/631e8b200cce187082ee46b0eb1681b3_152_0.png)

这通常意味着图像包含背景和前景像素，而大津法是在不指定阈值的情况下自动分离这两组像素的最佳方法。

对于不符合背景和前景模型的图像，大津法不是最佳方法，如果应用可能产生不正确的输出。

此方法与其他方法结合使用，阈值参数传递为0。尝试以下代码：

```python
ret,output=cv2.threshold(image,0,255,cv2.THRESH_BINARY+cv2.THRESH_OTSU)
```

其输出如下。这是沙漠中坦克的截图：

![](img/631e8b200cce187082ee46b0eb1681b3_153_0.png)

## 噪声

**噪声**意味着不需要的信号。图像/视频噪声是指强度（对于灰度图像）或颜色（对于彩色图像）的不期望变化，这些变化在正在拍摄或记录的实物中并不存在。图像噪声是一种电子干扰形式，可能来自多种来源，例如相机传感器以及数字或模拟相机的电路。数字相机中的噪声相当于模拟相机的胶片颗粒。尽管任何电子设备的输出中总是存在些许噪声，但大量的图像噪声会显著降低整体图像质量，使其无法达到预期目的。为了表示电子输出（在本文中即数字图像）的质量，数学术语**信噪比（SNR）**是一个非常有用的概念。其数学定义如下：

$$SNR = \frac{SignalPower}{NoisePower}$$

> 更高的信噪比意味着更好的图像质量。

### 用于噪声去除的核

在以下概念及其实现中，我们将使用核。**核**是图像处理中使用的方阵。我们可以将核应用于图像以获得不同结果，例如图像的模糊、平滑、边缘检测和锐化。核的主要用途之一是向图像应用低通滤波器。低通滤波器会平均掉图像像素强度的快速变化。这基本上会平滑或模糊图像。一个简单的平均核可以用数学公式表示如下：

$$K = \frac{All\ Ones\ Matrix}{Rows * Cols}$$

当 row = cols = 3 时，核如下所示：

$$K = \frac{\begin{bmatrix}1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1\end{bmatrix}}{9}$$

核中行和列的值始终为奇数。

我们可以使用以下NumPy代码创建这个核：

```python
K=np.ones((3,3),np.uint32)/9
```

### 2D卷积滤波

cv2.filter2D() 函数将前面提到的核与图像进行卷积，从而对图像应用线性滤波。该函数接受源图像、目标图像深度（在我们的示例中为-1，其中-1表示与源图像相同的深度）以及核。请看以下代码，它将一个7x7平均滤波器应用于图像：

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

img = cv2.imread('4.2.03.tiff',1)

input = cv2.cvtColor(img,cv2.COLOR_BGR2RGB)
output = cv2.filter2D(input,-1,np.ones((7,7),np.float32)/49)

plt.subplot(121),plt.imshow(input),plt.title('Input')
plt.xticks([]), plt.yticks([])
plt.subplot(122),plt.imshow(output),plt.title('Output')
plt.xticks([]), plt.yticks([])
plt.show()
```

输出将是一个滤波后的图像，如下所示：

![](img/631e8b200cce187082ee46b0eb1681b3_155_0.png)

### 低通滤波

正如核部分所讨论的，低通滤波器在去除尖锐分量（高频信息）方面非常出色，例如边缘和噪声，同时保留低频信息（即所谓的低通滤波），从而使图像模糊或平滑。

让我们探索OpenCV中可用的低通滤波函数。我们不必创建核并将其作为参数传递给这些函数；相反，这些函数会根据我们作为参数传递的核大小来创建核。

cv2.boxFilter() 函数以图像、深度和核大小为输入，并对图像进行模糊。我们可以将normalize指定为True或False。如果为True，核中的矩阵系数为1/(rows*cols)；因此，该矩阵称为归一化盒式滤波器。如果normalize为False，则系数为1，这将是一个非归一化盒式滤波器。非归一化盒式滤波器用于计算每个像素邻域上的各种积分特征，例如图像导数的协方差矩阵（用于密集光流算法等）。以下代码演示了归一化盒式滤波器：

```python
output=cv2.boxFilter(input,-1,(3,3),normalize=True)
```

代码的输出如下所示，由于核矩阵的尺寸较小，它的平滑程度将低于前一个输出：

![](img/631e8b200cce187082ee46b0eb1681b3_156_0.png)

cv2.blur() 函数直接提供归一化盒式滤波器，它接受输入图像和核大小作为参数，无需指定normalize参数。以下代码的输出将与上述输出完全相同：

```python
output = cv2.blur(input,(3,3))
```

