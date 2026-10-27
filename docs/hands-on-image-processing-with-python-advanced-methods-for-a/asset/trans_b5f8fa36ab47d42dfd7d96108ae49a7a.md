

# 数据类型之间的转换

```
im_float = im.astype(np.float32) / 255.0 # Normalize float [0, 1]
print("Float dtype:", im_float.dtype)
print("New range:", im_float.min(), "to", im_float.max())
# Float dtype: float32
# New range: 0.0 to 1.0
```

## 注意：溢出与裁剪

当对 uint8 图像进行操作时，进行算术运算需要格外小心：

```
bright = im + 100 # Wrong: causes overflow if dtype is uint8
```

这会因溢出而发生回绕（例如，250 + 100 = 94）。

以下是正确的做法：

```
bright = np.clip(image.astype(np.int16) + 100, 0, 255).astype(np.uint8)
```

下面这种做法甚至更好：

```
im_float = im.astype(np.float32) / 255.0
bright_float = np.clip(im_float + 0.4, 0.0, 1.0)
```

## 使用布尔图像生成二值掩码

在图像分割或阈值化处理中，你通常需要生成布尔掩码，如下一个代码片段所示：

```
gray = rgb2gray(image)  # Returns float image [0, 1]
mask = gray > 0.5        # Boolean mask
print("Mask dtype:", mask.dtype)  # Output: bool
# Mask dtype: bool
```

## 专业提示

使用 `skimage.img_as_float` 和 `skimage.img_as_ubyte` 来进行安全转换：

```
from skimage import img_as_float, img_as_ubyte
im_float = img_as_float(im)  # converts safely to float [0,1]
im_uint8 = img_as_ubyte(im_float)  # converts safely back to uint8
```

请记住，OpenCV-Python 在图像处理中主要使用 NumPy 数组。以下是一些常见的 OpenCV 数据类型与其对应的 NumPy 数据类型之间的映射关系：

- cv2.CV_8U（8位无符号整数）：numpy.uint8
- cv2.CV_32F（32位浮点数）：numpy.float32
- cv2.CV_64F（64位浮点数）：numpy.float64

# 颜色空间

颜色空间是利用坐标系对颜色进行数学表示的一种方式，它能够实现高效的图像表示、操作和分析。不同的颜色空间是为不同的图像处理任务而设计的，因为没有任何单一的表示方法对所有应用都是最优的。

常用的颜色空间包括以下几种：

- **RGB（红、绿、蓝）**：电子显示器和一般图像处理中使用的标准加色模型。
- **HSV（色相、饱和度、明度）**：将颜色、色彩饱和度和亮度分离，适用于颜色过滤、分割和颜色操作。
- **CMYK（青、品红、黄、黑）**：主要用于印刷和出版领域的减色模型。
- **Lab（CIELAB）**：使用亮度（L*）和对立色通道（a、b）来表示颜色。它在感知上是均匀的，广泛用于颜色校正和比较。
- **YUV**：分离亮度和色度分量，能够实现高效的视频压缩和广播。
- **YCbCr**：将亮度（Y）与色度（Cb、Cr）分离，广泛用于JPEG、MPEG和数字视频压缩中。
- **YIQ**：NTSC标准中使用的一种电视广播颜色模型，将亮度和色度信息分离。

不同的颜色空间会强调不同的图像特性，例如亮度、色度、感知均匀性或压缩效率。例如，RGB 直观且对硬件友好，HSV 简化了基于颜色的阈值处理，Lab 与人类颜色感知紧密对齐，YUV/YCbCr 提高了压缩效率，而灰度则降低了 OCR 和边缘检测等任务的计算复杂度。

选择合适的颜色空间取决于应用需求、计算需求以及与图像处理任务相关的感知特性。

我们可以使用仿射映射在颜色空间之间进行转换。下面的矩阵表示从 RGB 到 YIQ 颜色空间的线性映射：

RGB 到 YIQ
[ Y ]   [ 0.299   0.587   0.114 ] [ R ]
[ I ] = [ 0.596  -0.274  -0.322 ] [ G ]
[ Q ]   [ 0.211  -0.523   0.312 ] [ B ]

YIQ 到 RGB
[ R ]   [ 1   0.956   0.621 ] [ Y ]
[ G ] = [ 1  -0.272  -0.647 ] [ I ]
[ B ]   [ 1  -1.106   1.703 ] [ Q ]

Figure 1.18 – 颜色空间变换矩阵

# 颜色色域

颜色色域是指设备或系统能够重现或显示的颜色范围，通常表示为颜色空间中由原色定义的可见光谱的一个子集。更宽的色域能够实现更丰富、更准确的颜色再现，这在摄影、成像和显示技术等应用中非常重要。了解颜色色域有助于在不同设备和媒体之间保持一致且忠实的颜色表示。

**colour-science** Python 库可用于通过 `plot_chromaticity_diagram_CIE1931()` 函数，借助 **CIE 1931 色度图** 可视化颜色色域。使用 `pip install colour-science` 安装该包后，该函数会生成一个二维色度图，使用 x 和 y 色度坐标表示可感知的颜色。该图在颜色科学、显示技术、照明和图像处理应用中被广泛使用。

```python
# pip install colour-science
from colour.plotting import plot_chromaticity_diagram_CIE1931
plot_chromaticity_diagram_CIE1931(); plt.show()
```

如果你运行前面的代码，它将会生成如下所示的一幅输出图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_62_0.png)

*Figure 1.19 – CIE 1931 色度图*

# 色度图的应用

CIE 色度图在颜色科学和技术领域有着广泛的实际应用，包括以下几种：

- **颜色显示校准：** 该图通过将设备（例如显示器）的色域与人类感知进行比较，帮助校准设备。
- **照明体与光源比较：** 该图可以显示不同照明体或光源的色度坐标。
- **颜色研究：** 有助于研究颜色如何被感知，以及不同光照条件如何影响颜色外观。

# 在颜色空间之间进行转换

我们可以使用库函数在颜色空间之间进行转换。例如，下一小节中的代码使用 `color.rgb2hsv()` 函数，将 RGB 颜色空间转换为 HSV 颜色空间图像。

## RGB 转 HSV

HSV 颜色空间通常更适合用于颜色过滤或对象跟踪等任务，因为这样可以更轻松地分离颜色属性。下图对 RGB 和 HSV 颜色空间进行了比较。RGB 立方体（左）将颜色表示为红、绿、蓝强度的组合，形成一个立方体，每个轴对应一个颜色通道。HSV 圆柱体（右）则更符合人类感知的方式对颜色进行建模，其中 *Hue*（色调）为角度，*Saturation*（饱和度）为半径，*Value*（明度）为高度，如下图所示：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_63_0.png)

*Figure 1.20 – RGB 与 HSV 颜色空间对比*

我们可以使用下一个代码片段中的 `color.rgb2hsv()` 函数，将 RGB 图像转换为 HSV 颜色空间。下图显示了通过运行以下代码片段创建的鹦鹉 HSV 图像：

```
im = imread("images/parrot.png")
im_hsv = color.rgb2hsv(im)
# plot 3 channels of im_hsv here (e.g., im_hsv[...,0]) side-by-side here
```

如果你运行上述代码片段，你应该会得到类似下图的结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_63_1.png)

*Figure 1.21 – 显示 HSV 颜色空间中的色相、饱和度和明度通道*

## RGB 转 YUV

类似地，我们可以使用下面的代码，借助 color.rgb2yuv() 函数将 RGB 颜色空间转换为 YUV 颜色空间：

```
im_yuv = color.rgb2yuv(im)
imsave('images/parrot_yuv.png', (255*im_yuv).astype(np.uint8))
```

下图显示了使用上述代码创建的鹦鹉 YUV 图像，以及来自 RGB 颜色空间的原始红、绿、蓝通道：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_64_0.png)

Figure 1.22 – 显示 YUV 与 RGB 通道的对比

## RGB 转 CMYK

与 RGB（加色颜色空间）相反，CMYK 是一种减色光谱。这意味着这些油墨会在较浅的背景下（如白纸）遮蔽颜色。CMYK 油墨从白光中减去红、绿、蓝，留下青、品红和黄。黑色是颜色的缺失。RGB 颜色在屏幕上可能看起来很棒，但在将你的美术文件提供给打印机之前，需要将它们转换为 CMYK 颜色空间。

下图显示了从 RGB 转换为 CMYK 的公式：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_64_1.png)

Figure 1.23 – 从 RGB 到 CMYK 颜色空间的转换公式

