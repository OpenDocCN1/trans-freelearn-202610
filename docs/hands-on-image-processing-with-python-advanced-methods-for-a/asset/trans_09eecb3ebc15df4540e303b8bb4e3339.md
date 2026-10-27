

将 RGB 图像转换为 Lab，修改其中一个通道，再将其转换回 RGB：

```python
def add_channel_lab(im, i, x):
    im_lab = color.rgb2lab(im)
    im_lab[...,i] = x + im_lab[...,i]
    return color.lab2rgb(im_lab)

im = imread('images/scene.jpg')
plot_images([im, add_channel_lab(im, 0, 50), \
             add_channel_lab(im, 1, 10), \
             add_channel_lab(im, 2, -25)], \
            ['original', 'luminosity changed', 'red tinted', 'cooled'])
```

如果运行上述代码片段，您应该会得到类似下图的结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_70_0.png)

*图 1.31 – Lab 颜色空间中的颜色偏移*

## 使用 HSV 和 YCbCr 颜色空间进行肤色检测

肤色检测识别不同颜色空间中位于预定义肤色范围内的像素。以下实现同时使用 HSV 和 YCbCr 颜色空间以提高鲁棒性。输入 BGR 图像通过 `cv2.cvtColor()` 进行转换，并使用 `cv2.inRange()` 根据预定义的上下阈值生成二值肤色掩码：

$$HSV_{lower}, HSV_{upper},\quad YCbCr_{lower}, YCbCr_{upper}$$

处于指定范围内的像素被标记为肤色（1），而其他像素被抑制（0）。形态学开运算（`cv2.morphologyEx()`）去除小的噪声区域，`cv2.medianBlur()` 则对最终掩码进行平滑。使用 `np.bitwise_and()` 组合这两个掩码，以获得更可靠的全局肤色掩码。

函数 `detect_skin_color()` 接受一个 BGR 图像（`np.ndarray`），并返回原始 RGB 图像以及基于 HSV、基于 YCbCr 和组合的肤色检测输出。类型提示提高了可读性，并有助于编辑器辅助的错误检查，尽管它们在 Python 中是可选的。

最终的可视化使用 Matplotlib 将原始图像与来自 HSV、YCbCr 以及组合掩码的检测结果并排显示。

```python
# Constants for HSV and YCrCb skin detection thresholds
HSV_LOWER, HSV_UPPER = (0, 15, 0), (17, 170, 255)

YCrCb_LOWER, YCrCb_UPPER = (0, 135, 85), (255, 180, 135)

def detect_skin_color(img: np.ndarray) -> tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]:
    img_HSV = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    HSV_mask = cv2.inRange(img_HSV, HSV_LOWER, HSV_UPPER)
    HSV_mask = cv2.morphologyEx(HSV_mask, cv2.MORPH_OPEN, np.ones((3,3), np.uint8))
    img_YCrCb = cv2.cvtColor(img, cv2.COLOR_BGR2YCrCb)
    YCrCb_mask = cv2.inRange(img_YCrCb, YCrCb_LOWER, YCrCb_UPPER)
    YCrCb_mask = cv2.morphologyEx(YCrCb_mask, cv2.MORPH_OPEN, np.ones((3,3), np.uint8))
    global_mask = np.bitwise_and(YCrCb_mask,HSV_mask)
    global_mask = cv2.medianBlur(global_mask,3)
    global_mask = cv2.morphologyEx(global_mask, cv2.MORPH_OPEN, np.ones((4,4), np.uint8))
    img_RGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    out_HSV = cv2.cvtColor(img & cv2.merge([HSV_mask]*3), cv2.COLOR_BGR2RGB)
    out_YCrCb = cv2.cvtColor(img & cv2.merge([YCrCb_mask]*3), cv2.COLOR_BGR2RGB)
    out_global = cv2.cvtColor(img & cv2.merge([global_mask]*3), cv2.COLOR_BGR2RGB)
    return img_RGB, out_HSV, out_YCrCb, out_global

img = cv2.imread("images/cycling.png")
img_RGB, out_HSV, out_YCrCb, out_global = detect_skin_color(img)
plt.imshow(np.hstack((img_RGB, out_HSV, out_YCrCb, out_global)))
```

如果运行上述代码片段，您将得到如下图所示的输出：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_71_0.png)

*图 1.32 – 使用不同颜色空间进行肤色检测*

> **注意**
>
> 该函数明确使用了**类型提示**（例如，`img: np.ndarray` 和 `tuple[np.ndarray, ...]` 返回类型）。类型提示（Python 3.5 引入）提高了代码可读性，支持在现代编辑器（如 VS Code 或 PyCharm）中进行更好的错误检查和自动补全，并使函数的输入输出预期更加清晰，尤其在协作或大型项目中。然而，由于 Python 本质上是一种动态类型语言（即类型在运行时解析），类型提示是可选的，不会影响代码在运行时的功能。

正如我们已经讨论过的，PIL 使用 Image 对象来存储图像，而 scikit-image 使用 NumPy ndarray 数据结构来存储图像数据。下一节将介绍如何在这两种数据结构之间进行转换。

## 转换图像数据结构

以下代码块展示了如何从 PIL Image 对象转换为 NumPy ndarray（供 scikit-image 使用）：

```python
im = Image.open('images/flowers.jpg')  # read image into Image object (PIL)
im = np.array(im)  # create a numpy ndarray from the Image object
```

以下代码块展示了如何从 NumPy ndarray 转换为 PIL Image 对象。运行后，代码显示的输出与前述图形相同：

```python
im = imread('images/flowers.png')  # read image into numpy ndarray
im = Image.fromarray(im)  # create a PIL Image object from the numpy ndarray
#im.show() # display the image with PIL Image.show() method
```

现在我们已经了解了如何将图像加载到内存中，接下来介绍一些基本的图像处理方法。

## 图像数组坐标与绘图坐标

在 Python 中处理图像时，重要的是要理解**数组索引**和**绘图坐标**在表示空间数据时并不总是一致的。这种差异在可视化或处理图像时可能会引起混淆，尤其是在像素操作、绘图或叠加图形等任务中。

### 图像数组坐标（NumPy 风格）

在 NumPy 中，图像被视为多维数组。这意味着：

- 第一个索引对应于行（Y 轴），方向为从上到下；第二个索引对应于列（X 轴），方向为从左到右；原点 `(0,0)` 位于图像的左上角像素；Y（行）向下增大；X（列）向右增大。

### 绘图坐标（Matplotlib 风格）

Matplotlib 的常规绘图函数（例如 `plt.plot()` 和 `plt.scatter()`）使用标准笛卡尔坐标：原点 `(0,0)` 默认位于左下角，X 向右增大，Y 向上增大。然而，使用 `plt.imshow()` 显示图像时，图像仍按从上到下的方式显示（原点在左上角），除非另行指定。下表总结了坐标系之间的差异：

| 特性 | NumPy 数组 | Matplotlib 绘图 |
|---|---|---|
| 索引格式 | (row, col) = (y, x) | (x, y) |
| 原点（默认） | 左上 | 左下 |
| Y 轴方向 | 从上到下 | 从下到上（除非 origin='upper'） |
| 用途 | 处理与过滤 | 显示与叠加 |

表 1.4：numpy 二维数组与 matplotlib 绘图的坐标系

## 基本图像处理

不同的 Python 库可用于基本图像处理。几乎所有库都将图像存储在 NumPy ndarray 中（例如，灰度图像为二维数组，RGB 图像为三维数组）。如前所述，下图显示了彩色蒙娜丽莎图像的正 x 和 y 方向（原点位于图像二维数组的左上角）：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_73_0.png)

*图 1.33 – 图像与 NumPy 二维数组坐标的对比*

### 常见的图像处理操作

数字图像处理涉及对图像进行修改以增强外观、改变构图或创建视觉效果。常见操作包括裁剪、缩放、旋转、颜色调整、锐化、模糊、变形以及应用滤镜效果（如棕褐色或灰度）。更高级的操作包括添加文本、移除对象、克隆图像区域以及将多张图像合成为合成场景。本书将使用不同的 Python 图像处理库来实现这些技术中的许多内容。

### 使用 NumPy 进行图像处理

NumPy 是 Python 中用于数值和数组运算的强大库，凭借其数组处理能力，也可用于图像处理。以下小节将介绍一些使用 NumPy 的基本图像处理任务。

#### 使用数组切片进行裁剪

NumPy 数组切片是指提取数组的一部分或创建包含其部分元素的原始数组视图的过程。下面的代码片段展示了如何使用 NumPy 数组的切片和掩码操作，先从输入的蒙娜丽莎图像中裁剪出面部区域，然后通过应用圆形掩码隐藏图像的一部分：

```python
mona = plt.imread("images/monalisa.jpg") # read from disk as numpy ndarray
lx, ly = mona.shape[:2]
cropped_mona = mona[120:210, 160:220, :]  # crop a region
X, Y = np.ogrid[0:lx, 0:ly]
mask = (X - lx / 2) ** 2 + (Y - ly / 2) ** 2 > lx * ly / 4
masked_mona = mona.copy()
masked_mona[np.where(mask)] = 0 # masks
# plot mona, cropped_mona and masked_mona side-by-side here
```

如果运行上述代码片段，您应该会得到类似下图的结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_74_0.png)

*图 1.34 – 使用 NumPy 数组切片裁剪蒙娜丽莎图像*

#### 调整彩色图像的亮度和对比度

对比度和亮度调整是帮助增强图像或修正曝光不足的重要操作。对比度调整放大亮部与暗部之间的差异，而亮度调整则改变图像的整体亮度。亮度和对比度的调整通常在 Lab 颜色空间中进行，该空间将亮度与颜色信息分离。RGB 图像使用 `rgb2lab()` 转换为 Lab，通过亮度通道（L）进行修改，再使用 `lab2rgb()` 转换回 RGB。

