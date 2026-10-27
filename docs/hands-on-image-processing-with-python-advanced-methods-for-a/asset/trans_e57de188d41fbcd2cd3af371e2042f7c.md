

如我们之前所见，滤镜使用数学变换修改像素值，例如颜色映射、伽马校正和卷积。滤镜通常涉及以下内容：

- **颜色变换：** 使用变换矩阵调整 RGB 值：

$$
\begin{bmatrix}R'\\G'\\B'\end{bmatrix}=M\times\begin{bmatrix}R\\G\\B\end{bmatrix}+B
$$

这里，$M$ 是变换矩阵，$B$ 是偏置向量。

- **伽马校正：** 使用 $I' = I^\gamma$ 调整亮度，其中 $\gamma$ 控制亮度。

- **卷积滤镜：** 应用核 $K$ 来增强特征：

$$
I'(x,y)=\sum_{i=-k}^{k}\sum_{j=-k}^{k}I(x-i,y-j)K(i,j)
$$

这里，$K$ 是滤波器核，例如高斯模糊或锐化。

以下 Python 代码片段将 Pilgram 库中的多个 Instagram 风格滤镜应用于图像并以网格形式显示。以下是关键步骤：

1. `getmembers(pilgram, isfunction)` 函数检索 Pilgram 库中可用的所有滤镜函数。
2. 莫奈丽莎图像从磁盘加载，然后每个滤镜函数都应用于此（原始）图像 im，并将生成的图像存储在名为 effects 的字典中，以滤镜名称作为键。
3. 原始图像以及经过滤波的图像（以及作为标题的已应用滤镜名称）通过遍历 effects 字典，使用 Matplotlib 以网格形式显示。

现在，参考以下代码片段：

```
# !pip install pilgram
import pilgram
# dir(pilgram)
from PIL import Image
from inspect import getmembers, isfunction
import matplotlib.pyplot as plt

im = Image.open('images/monalisa.jpg')
effects = {'original': im}
for f in getmembers(pilgram, isfunction):
```

```
```python
effects[f[0]] = f[-1](im)

i = 1
for e, im in effects.items():
    # print(e)
    plt.subplot(5,6,i), plt.imshow(im), plt.title(e, size=15)
    i += 1
```
```

如果运行上述代码片段，您应该得到类似以下图示的结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_182_0.png)

*图 3.34 – 使用 Pilgram 的 Instagram 风格滤镜*

## 使用 pilgram.css 模块

Pilgram 库使用 Python 重现 Instagram 滤镜，遵循与 CSS 滤镜相同的图像处理技术。虽然 CSS 滤镜在 Web 浏览器中应用，但 Pilgram 使用 PIL（Pillow）在 Python 中以编程方式应用它们。

下一个代码片段将五个 CSS 风格滤镜（contrast、grayscale、hue_rotate、saturate 和 sepia）从 pilgram.css 应用于图像，并使用 Matplotlib 并排显示：

```
```python
import pilgram.css
im = Image.open('images/me.png')

i = 1
for f in ['contrast', 'grayscale', 'hue_rotate', \
          'saturate', 'sepia']:
    func = getattr(pilgram.css, f)
    plt.subplot(1,5,i), plt.imshow(func(im))
    plt.title(f, size=12)
    i += 1
```
```

如果运行上述代码片段，您应该得到类似以下结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_183_0.png)

*图 3.35 – 使用 Pilgram 的 CSS 风格滤镜*

## 实际应用案例亮点

本章讨论的技术虽然强调视觉效果和创意操控，但也在各个行业中发挥着实际作用。例如，伽马校正和基于 LUT 的色彩分级是电影后期制作工作流程的标准；晕影和色温调整用于移动照片编辑应用；鱼眼和去畸变技术对于 AR 和 VR 应用至关重要，因为相机校准至关重要；波浪失真或素描效果可以增强视频游戏视觉效果或动画内容。将这些效果纳入生产流程不仅增强了美学质量，还提高了实时应用（如滤镜、交互式媒体和增强体验）中的用户参与度。

## 总结

在本章中，我们使用 PIL、opencv-python、SciPy、Wand、Matplotlib 和 Pilgram 等各种 Python 库探索了高级图像处理技术。我们涵盖了使用 LUT 文件进行的基础变换，例如调整大小、旋转、翻转和颜色调整。此外，我们还应用了伽马校正、镜头模糊、径向模糊、晕影和洛莫效果来增强图像美感。

我们还尝试了单应性、鱼眼模拟、波浪失真和仿射变换进行几何修改。此外，我们还实现了 Instagram 风格滤镜、轮廓映射和注释技术来美化图像。通过掌握这些技术，您可以以编程方式操纵和增强图像，为计算机视觉和数字图像处理中的创意和实际应用开辟可能性。

下一章，我们将深入探讨数字信号处理的基础主题，即采样和傅里叶变换。

## 获取本书的 PDF 版本和更多内容

扫描二维码（或访问 packtpub.com/unlock）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_184_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_184_1.png)

注意：请保留您的发票。直接从 Packt 购买无需发票。

# 4

# 采样和傅里叶变换

在本章中，图像被研究为空间域和频率域中的 2D 信号。本章从 **空间采样和量化** 开始，包括图像调整大小、混叠和奈奎斯特-香农采样准则，以及使用 Python 库的实际实现。还讨论了强度量化及其对图像质量和存储的影响。

本章介绍了用于将图像从空间域转换到频率域的 **离散傅里叶变换（DFT）**。您将学习如何使用 numpy 和 scipy 提供的 **快速傅里叶变换（FFT）** 算法高效计算 DFT 和逆 DFT，分析图像频率谱，并研究重要的 DFT 属性，如线性、可分离性和帕塞瓦尔能量守恒定理。还介绍了相关的变换，包括 **离散余弦变换（DCT）** 和 **沃尔什-哈达玛变换（WHT）**。

我们将涵盖以下关键主题：

- 图像形成 – 采样和量化
- 离散傅里叶变换

## 技术要求

以下是本书配套 GitHub 仓库中本章 Jupyter 笔记本的链接：https://github.com/PacktPublishing/Hands-On-Image-Processing-and-Computer-Vision-with-Python-Second-Edition。 在阅读本章的不同部分时，请跟随此笔记本（并运行必要的代码片段）。在运行代码之前，必须安装正确版本的库才能无缝运行。如果版本不匹配，代码可能需要根据库中某些功能/函数是否被移除/弃用进行少量调整。

## 图像形成 — 采样和量化

图像形成从根本上涉及 **采样** 和 **量化**。采样是空间操作，选择图像像素以改变图像分辨率，而量化则减少用于表示图像的强度或颜色级别的数量。这些操作构成了图像调整大小、压缩和数字图像表示的基础。采样可用于增加或减少图像大小，分别对应上采样和下采样。

## 上采样

图像可被视为连续 2D 强度函数 f(x, y) 的离散采样。上采样通过在强度值未知的地点生成额外像素来增加图像分辨率（大小）。由于这些值在原始图像中不可用，因此必须使用均值等聚合统计或基于邻近像素的插值技术来估计。插值试图重建底层图像信号的连续近似，同时保持视觉平滑性和结构连续性。给定采样图像点 f(x_i, y_i)，插值估计任意空间位置 (x, y) 处的强度值。常见的插值技术包括最近邻、双线性、双三次和样条插值。

## 最近邻插值

最近邻插值是最简单的上采样技术。重采样图像中的未知像素值被赋予原始图像中最接近像素的值：

I'(x', y') = I(x, y)

其中 (x, y) 是原始图像中对应于重采样坐标 (x', y') 的最近像素位置。坐标映射使用缩放因子 s_x 和 s_y 执行：

x = round( x' / s_x ),    y = round( y' / s_y )

其中 round() 选择最近的整数坐标。例如，双倍图像尺寸会复制相邻的行和列。下图显示了使用最近邻方法上采样的示例，其中图像高度和宽度加倍（S_x = S_y = 2），因此我们有 I'(x, y) = I( [x/2], [y/2] )：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_186_0.png)

图 4.1 – 使用最近邻插值的上采样

尽管计算高效，但最近邻插值通常会产生块状和锯齿状伪影，因为没有生成新的强度值。

## 双线性插值

双线性插值通过使用目标位置周围的四个最近邻像素来改善平滑度。它沿 x 和 y 方向依次执行线性插值。

