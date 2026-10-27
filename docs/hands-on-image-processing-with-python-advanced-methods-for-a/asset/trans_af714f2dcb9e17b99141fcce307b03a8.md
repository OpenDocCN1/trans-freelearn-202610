

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_227_0.png)

图4.47 – 展示DFT线性性质的图像重建

## 旋转

将输入图像旋转一个角度，意味着频谱也会旋转相同的角度，如下图所示：

f(r, θ) ↔ F(ω, φ)
f(r, θ + α) ↔ F(ω, φ + α)
以角度α旋转

图4.48 – DFT的旋转性质

下面的代码片段演示了这一性质：

```python
from scipy.ndimage import rotate
im = imread('images/box.png', True)
F = np.fft.fftshift(np.fft.fft2(im))
im_rot = rotate(im, 45)
F_rot = np.fft.fftshift(np.fft.fft2(im_rot)) # rotated spectrum
# plot original & rotated images (im, im_rot) and spectrums (F, F_rot)
```

如果运行前面的代码片段并绘制图像，你应该会得到类似下图的输出：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_228_0.png)

图4.49 – 图像旋转对DFT频谱的影响

## 相似性/缩放

拉伸图像会导致频谱收缩（反之亦然），如下图所示：

f(x, y) ↔ F(u, v).

f(αx, βy) ↔ 1/|αβ| F(u/α, v/β)

图4.50 – DFT的缩放性质

下面的代码片段使用拉伸的矩形来演示这一性质：

```python
f_small = imread('images/box.png', True)
F_small = np.fft.fftshift(np.fft.fft2(f_small))
f_large = imread('images/box_stretched.png', True) # stretched image
F_large = np.fft.fftshift(np.fft.fft2(f_large)) # stretched spectrum
# plot images (f_small, f_large) and spectrums (F_small, F_large) here
```

如果运行上述代码并绘制图像，你应该会得到类似下图的输出：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_230_0.png)

图4.53 – 空间域中的平移对DFT频谱的影响

## 能量守恒：帕塞瓦尔/瑞利定理

帕塞瓦尔/瑞利定理表明了空间域和频率域之间的能量守恒，对于一个二维信号（图像），其表述如下：

下面的代码片段演示了该定理对lena图像成立：

```python
f = imread('images/lena.jpg', True)
F = np.fft.fft2(f)
f_energy, F_energy = np.sum(np.abs(F)**2) / np.prod(f.shape), np.sum(f**2)
f_energy, F_energy, np.isclose(f_energy, F_energy)
# (11918.093481563103, 11918.0934815631, True)
```

从上面的输出可以看出，在空间域和频率域中计算得到的能量非常接近，从而证实了能量在不同域间的守恒。

## 小结

在本章中，我们讨论了一些主要与二维DFT及其在图像处理中的相关应用有关的重要概念，例如频域滤波，并使用scikit-image、numpy.fft、scipy.fftpack、scipy.signal和scipy.ndimage等模块完成了不少示例。

希望你现在已经清楚采样和量化——这两种重要的图像形成技术。我们已经了解了二维DFT、FFT算法的Python实现，以及DFT在图像处理中的应用，如图像去噪与恢复、相关和卷积。你还了解了在滤波器设计中使用适当核进行卷积的应用，以及相关在模板匹配中的应用。现在你应该能够编写Python代码，使用PIL/SciPy/scikit-image库进行采样和量化，并使用FFT算法在Python中执行二维FFT/IFFT。我们还看到，使用一些核对图像进行基本二维卷积是多么容易。

在下一章中，我们将介绍卷积——图像处理/计算机视觉中另一个基本操作——并探讨其广泛的应用范围。我们还将学习频域滤波以及各种频域滤波器。

## 获取本书PDF版本及更多内容

扫描二维码（或访问 packtpub.com/unlock）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_231_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_231_1.png)

注意：请随身携带发票。直接从Packt购买的书籍不需要发票。

## 5 卷积与空间/频域滤波

在本章中，我们将探讨图像处理中最基本的操作之一：卷积。二维卷积在从边缘检测到图像平滑和特征提取的广泛应用中发挥着核心作用。我们将首先建立对卷积的直观和数学理解，并解释为什么它在处理视觉数据时如此关键。然后，我们将区分空间域卷积和频域卷积，强调后者如何利用卷积定理显著加速计算。通过详细的比较、可视化以及使用scipy.signal、scipy.ndimage、opencv-python和numpy.fft等库的Python实现，我们将看到这些技术在实际中如何应用。

在此过程中，我们还将澄清相关（correlation）和卷积之间常被混淆的区别，并演示如何使用相关进行模板匹配——这是计算机视觉中的一项重要任务。我们将实现互相关（cross-correlation）和归一化互相关（NCC）来定位图像中的模式，并比较不同方法的性能。最后，本章将以二维与三维卷积的讨论作为结尾，深入探讨这些概念如何扩展到体积数据。无论你是在空间域还是频域工作，到本章结束时，你都将对卷积及其在现代图像处理中的强大作用有一个实用的、动手实践的理解。本章将涵盖以下主题：

- 卷积定理及其应用
- 频域滤波（使用scipy.ndimage模块和scikit-image）
- 相关与卷积
- 使用归一化互相关（NCC）进行模板匹配
- 二维与三维卷积

## 技术要求

以下是本书随附的GitHub仓库中与本章对应的Jupyter笔记本链接：https://github.com/PacktPublishing/Hands-On-Image-Processing-and-Computer-Vision-with-Python-Second-Edition。在阅读本章各个部分时，请跟随此笔记本（并运行必要的代码片段）。为了确保代码按预期运行，并且不受未来弃用或向后不兼容更改的影响，仓库的同一文件夹中提供了一个requirements.txt文件——使用pip install -r requirements.txt安装它。

## 理解卷积

（二维）卷积是一种作用于两幅图像的操作，其中一幅是输入图像，另一幅是掩膜（也称为核，kernel），作为输入图像上的滤波器，产生一幅输出图像。卷积滤波用于修改图像的空间频率特性。它的工作原理是，通过将中心像素的所有相邻像素的加权值相加来确定中心像素的值，从而计算输出图像中该像素的新值。输出图像中的像素值是通过让核窗口遍历输入图像来计算的，如下面的截图所示（针对使用valid模式的卷积；我们将在本章后面看到卷积模式）：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_234_0.png)

图5.1 – 使用valid模式将图像与核进行卷积

如你所见，核窗口在输入图像中用箭头标出，它遍历图像，并在卷积后获得映射到输出图像上的值。

## 为什么对图像进行卷积？

卷积是图像处理中的一项基本操作，用于多种目的，例如滤波、特征检测和平滑。当我们用核（也称为滤波器）对图像进行卷积时，核被应用于图像中的每个像素及其相邻像素，以在输出图像中为该像素生成一个新值。

