

以下是进行图像卷积的原因以及其在图像处理中至关重要的原因：

- **数学基础：** 卷积在数学上将输入图像与核结合，以提取或抑制某些特征
- **计算效率：** 大多数图像处理算法，尤其是在计算机视觉中，由于其简单性和效率而严重依赖卷积
- **通用性：** 卷积是一种通用操作，只需更改核即可适应各种任务，从而可以在图像上实现各种效果和变换

它在图像处理和深度学习中广泛用于特征提取和滤波。在本节中，我们将使用卷积对图像应用通用滤波器效果。这是为了通过适当的核在图像上实现各种效果，例如平滑、锐化和浮雕，以及诸如边缘检测之类的操作。然而，根据卷积是在空间域还是频率域执行，计算效率可能会显著变化。

## 空间域卷积

2D 空间卷积的核心是滑动一个 **kernel**（或滤波器）覆盖图像，以计算每个位置周围像素强度的 **weighted sum**。在数学上，对于大小为 M × N 的图像和大小为 k × k 的 **filter**（核）K，像素位置处的卷积计算如下：

O(x, y) = Σₘ Σₙ K(m, n) · I(x − m, y − n)

虽然很直接，但对于大图像或大核尺寸，这种方法在计算上变得昂贵，因为操作次数随核尺寸呈二次增长。

**时间复杂度（空间域）：** 空间域卷积的时间复杂度为 O(k²MN)，其中 k 是核尺寸，M × N 是图像尺寸。对于小核尺寸（例如 3x3 或 5x5），这是可以管理的，但对于大核，成本变得难以承受。

## 频率域卷积

在频率域中，利用 **convolution theorem** 可以更有效地执行卷积，该定理指出空间域中的卷积等价于频率域中的逐元素乘法。步骤如下：

- 使用 **Fast Fourier Transform (FFT)** 将图像 I 和核 K 变换到频率域。
- 逐元素相乘傅里叶变换后的图像和核。
- 反转 FFT 将结果转换回空间域。

在数学上，如果 F 表示傅里叶变换，则卷积计算如下：

F(I * K) = F(I) · F(K)

主要优势在于 FFT 可以更有效地计算卷积，尤其对于大核，因为 FFT 将卷积的时间复杂度从二次降低到接近线性。

## 时间复杂度

傅里叶基图像处理步骤的总体计算复杂度可分解如下：

- 大小为 $M \times N$ 的图像的 **FFT** 可以在 $O(MN \log(MN))$ 中计算
- 傅里叶变换的逐元素乘法的时空复杂度为 $O(MN)$
- **Inverse FFT (IFFT)** 也需要 $O(MN \log(MN))$

因此，频率域中的总体时间复杂度为：$O(MN \log(MN))$。这比空间域中二次时间 $O(k^2 MN)$ 更有效，尤其对于大图像和大核。

## 比较 — 空间与频率域

为了决定何时使用空间或频率域滤波，有助于根据核尺寸和计算效率比较其性能特征。以下要点总结了主要差异：

- 对于小核，空间域卷积通常更快，因为执行 FFT 的开销超过了逐元素乘法的益处。这就是为什么直接卷积用于典型图像滤波器中的 $3 \times 3$ 或 $5 \times 5$ 等核。
- 对于大核，频率域方法更有效。$O(MN \log(MN))$ 的复杂度比 $O(k^2 MN)$ 增长更慢，尤其当 $k$ 变大时。

> **Note**
> 虽然基于 FFT 的卷积对于大核是高效的，但对于非常大的图像可能会变得内存密集。在这种情况下，可以使用诸如 *tiled FFT*、*overlap-save* 或 *overlap-add* 等高级策略，以小块处理图像，而不会超出内存限制。

## 实际应用

空间域和频率域卷积之间的选择取决于效率和任务。使用 Sobel 和 Prewitt 等小核算子的边缘检测通常在空间域中执行，以实现实时性能。使用大核的模糊和平滑，例如高斯模糊，通常在频率域中实现以降低计算成本。用于天文图像恢复和生物医学显微镜等应用的去卷积也通常依赖于频率域技术（例如 Wiener 和 **Richardson–Lucy deconvolution**）来从模糊观测中高效恢复图像细节。

## 在空间域中应用卷积

现在，让我们演示如何使用来自不同库的以下函数将 2D 卷积应用于图像。

### With `scipy.signal`

`scipy` 库的 signal 模块的 `convolve2d()` 函数可用于对图像应用 2D 卷积。让我们使用此函数对图像（首先是灰度图像，然后是 RGB 彩色图像）与核（也称为滤波器，或有时称为点扩散函数 – PSF）应用卷积。

### Convolution on grayscale Images (SciPy)

让我们现在演示图像处理中的两个基本操作：使用盒式模糊滤波器 **blurring** 图像（此处使用灰度 cameraman 图像作为输入图像）以及使用 Laplace 核 **detecting edges**，使用以下代码片段通过 2D 卷积实现。以下是您将在下面找到的代码相关部分的详细解释：

1. **Defining the box blur kernel**
    - 创建一个盒式模糊核，即 3x3 的全 1 矩阵。除以 9 **normalizes** 核，使核中所有值的总和为 1。这确保卷积不会放大图像的亮度。
    - 应用于图像时，此核会平均 3x3 邻域内的像素值，从而产生平滑或模糊效果。

2. **Defining the Laplace edge detection kernel**
    - **Laplace kernel** 用于执行 **edge detection**。它通过计算图像的二阶导数来检测强度变化。核突出显示强度显著变化的区域，这通常对应于边缘。
    - 在此核中，中心像素的权重为 -4，而其四个方向相邻像素的权重为 +1。这种排列强调像素强度快速变化（即边缘）。

3. **Applying convolution to the image:**
    - `signal.convolve2d()` 将灰度图像 `im` 与核进行卷积。卷积将核在整个图像上滑动，用邻居的加权平均值替换每个像素，从而计算卷积图像。
    - `blur_box_kernel` 对输入图像应用平滑/模糊处理，结果存储在 `im_blurred` 中。
    - `edge_laplace_kernel` 计算像素与其邻居之间的像素强度差异，突出显示强度急剧变化的区域（即边缘）。

现在，参考以下代码片段：

```python
%matplotlib inline
from PIL import Image
from skimage.io import imread, imshow, show
import scipy.fftpack as fp
import numpy.fft as nfp
from scipy import ndimage, misc, signal
from skimage import data, color, restoration, img_as_float
from skimage.color import rgb2gray
from skimage.transform import rescale
from skimage.metrics import peak_signal_noise_ratio as psnr
import matplotlib.pylab as plt
from matplotlib.colors import LogNorm
import numpy as np
import numpy.fft
import timeit
import warnings
warnings.filterwarnings('ignore')
im = rgb2gray(imread('images/cameraman.jpg'))
print(np.max(im))
# 0.9921568627450981
print(im.shape)
# (256, 256)
blur_box_kernel = np.ones((3,3)) / 9 # normalize the kernel
edge_laplace_kernel = np.array([[0,1,0],[1,-4,1],[0,1,0]])
im_blurred = signal.convolve2d(im, blur_box_kernel)
im_edges = np.clip(signal.convolve2d(im, edge_laplace_kernel), \
                                  0, 1)
```

如果运行前面的代码片段并绘制使用 scipy.signal 模块的 convolve2d() 函数获得的卷积图像以及原始图像，您应该得到如下图所示：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_238_0.png)

*Figure 5.2 – Application of box blur and Laplace filter on cameraman image*

执行卷积操作，例如对图像应用滤波器时，正确处理输入图像的边界（边缘像素）非常重要，并定义处理过程中图像的填充方式。有多种模式、填充值和边界条件会影响输出。这些可以通过向 scipy.signal convolve2d() 函数传递 mode、boundary 和 fillvalue 参数来指定。

