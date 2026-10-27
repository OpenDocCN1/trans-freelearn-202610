
## 幂律变换

如我们所见，该点变换（传递函数）的一般形式为：

\[
s = T(r) = c r^\gamma
\]

其中 \(c\) 是常数。在第 1 章中我们使用 `PIL.Image.point()` 方法将其应用于灰度图像，这次我们用 scikit-image 在 RGB 彩色图像上应用幂律变换，并可视化该变换对颜色通道直方图的影响。

```python
im = img_as_float(imread('images/cheetah.png'))
γ = 2.5 # try different values here and observe the impact
im_trans = im**γ
```

如果运行上述代码片段，并绘制图像及相应的 RGB 通道直方图，您将得到下图，该图展示了幂律变换前后的颜色通道直方图，以及原始图像和变换后的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_308_0.png)

令像素值归一化到 \([0,1]\) 范围内。\(\gamma\) 的效果关键取决于它是小于 1 还是大于 1（如下表和下图所示）。

-   当 \(\gamma < 1\) 时，变换会扩展较低强度值，使暗区域变亮，提高阴影中的可见度。
-   相反，当 \(\gamma > 1\) 时，变换会压缩较低强度值，导致图像整体看起来更暗。

| Gamma | 曲线形状 | 效果 |
|---|---|---|
| \(\gamma < 1\) | 对角线以上 | 使阴影变亮 |
| \(\gamma = 1\) | 直线 | 无变化 |
| \(\gamma > 1\) | 对角线以下 | 使图像变暗 |

因此，与常见直觉相反，增大 \(\gamma\) 并不会使图像变亮，而是使其变暗；而小于 1 的值则产生变亮效果。该变换是非线性的：它重新分配对比度，而不仅仅是亮度。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_309_0.png)

*图 7.4 – 幂律变换*

## 亮度调整

亮度是图像最基本的属性之一。调整亮度在图像增强、预处理、视觉美感和可读性方面起着关键作用。在许多实际应用中，尤其是涉及低光、过曝或欠曝图像时，亮度校正是迈向更好图像理解或下游处理（如目标检测、OCR 或分类）的第一步。让我们探讨如何使用 PIL 和 opencv-python 等库调整图像亮度。

在数字成像中，亮度是指图像感知到的强度。对于灰度图像，它直接对应像素值。对于彩色图像，则更为微妙，通常与特定颜色空间（如 HSV、Lab 或 YUV）中的亮度或明度分量相关。对于图像 \(I(x, y)\)，亮度调整后的图像 \(I'(x, y)\) 通常按下式计算：

\[
I'(x, y) = \alpha \cdot I(x, y) + \beta
\]

其中 \(\alpha\) 是对比度缩放因子（对于仅调整亮度通常为 1），\(\beta\) 是亮度偏移量（正值变亮，负值变暗）。

由于数字图像以固定值范围表示（例如 8 位图像为 0–255），调整后的值必须进行裁剪：

\[
I'(x,y)=\min(255,\max(0,\alpha\cdot I(x,y)+\beta))
\]

下表列出了亮度调整非常有用的几个用例：

| 领域 | 目的 |
|---|---|
| 摄影 | 校正欠曝/过曝的照片 |
| OCR 预处理 | 提高文本可见性 |
| 监控 | 增强昏暗或夜间影像 |
| 卫星图像 | 在分析前改善对比度 |
| 医学成像 | 揭示扫描中的隐藏结构 |

*表 7.2 – 不同场景下的亮度调整*

### 使用 PIL

让我们首先通过以下代码片段探索如何使用 PIL 调整亮度，步骤如下：

1.  首先，使用 `Image.open()` 加载图像，并使用 `ImageEnhance.Brightness(img)` 创建亮度增强器。
2.  `enhance(1.5)` 方法将亮度提高 50%（其中 1.0 为原始亮度）。
3.  使用 matplotlib 并排显示原始图像和增亮图像以进行比较。

本质上，它演示了如何使用 PIL 快速调整图像亮度并可视化结果。现在参考以下代码片段：

```python
img = Image.open('images/boy.png')
enhancer = ImageEnhance.Brightness(img)
bright_img = enhancer.enhance(1.5)
dark_img = enhancer.enhance(0.5)
# plot images and intensity histogram here, refer to github code
```

如果运行上述代码并并排绘制图像和通道直方图，您将得到类似下图的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_311_0.png)

*图 7.5 – 使用 PIL 改变图像亮度*

它具有以下关键优点：使用直观，支持所有图像模式，并包含内置的伽马和色彩调整工具。

### 使用 OpenCV-Python

现在让我们使用以下代码片段，探索如何利用 opencv-python 通过仿射变换执行亮度调整。让我们通过以下逐步解释来理解代码：

-   首先使用 `cv2.imread()` 以 BGR 格式加载输入图像。使用函数 `cv2.cvtColor()` 将其转换为 RGB 格式，以便用 matplotlib 正确显示。
-   使用函数 `cv2.convertScaleAbs()` 对每个像素强度应用仿射变换：
    \[
    I'(x,y) = \alpha \cdot I(x,y) + \beta
    \]
-   \(\alpha\) 控制对比度：
    -   \(\alpha > 1\)：增大对比度（扩展强度范围）
    -   \(0 < \alpha < 1\)：降低对比度（压缩强度范围）
    -   \(\alpha = 1.0\)：保持原始对比度

```python
high_contrast_img = cv2.convertScaleAbs(img, alpha=2, beta=0)
# Brightness increase by adding constant
high_brightness_img = cv2.convertScaleAbs(img, alpha=1.0, beta=50)
plt.imshow(np.hstack((img, high_contrast_img, high_brightness_img)))
plt.axis('off')
```

如果运行上述代码片段，您将得到下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_313_0.png)

*图 7.6 – 使用 opencv-python 增加图像亮度*

该方法简单、高效，常用于实时图像增强。它通过 alpha 和 beta 同时支持亮度和对比度调整，适合批量处理，并在计算机视觉流程中广泛使用。

## 色调与感知增强

本节探讨基于强度的技术，如阈值处理、半色调化和抖动，这些技术通过修改像素色调来增强视觉感知，或为有限的显示和打印设备准备图像，包括广泛使用的 Floyd–Steinberg 误差扩散方法。

### 阈值处理

这是一种点运算，通过将低于某个阈值的所有像素设为 0，高于该阈值的所有像素设为 1，从灰度图像创建二值图像。如果 \(g\) 是 \(f\) 在某个全局阈值 \(T\) 下的阈值化版本，则可以表示为：

\[
g(x,y)=\begin{cases}1, & f(x,y)>T\\0, & \text{otherwise}\end{cases}
\]

为什么需要二值图像？原因有很多，例如，我们可能希望将图像分离为前景和背景；图像要用黑白打印机打印（所有灰度阴影需要仅用黑白点表示）；或者我们希望使用**形态学运算**对图像进行预处理，这将在后续章节中讨论。

以下代码片段展示了如何使用 `PIL.Image.point()` 方法进行固定阈值处理：

```python
im = Image.open('images/swans.jpg').convert('L')
threshold = [0, 50, 100, 150, 200]
for i in range(2, 5):
    im_bin = im.point(lambda x: x > threshold[i])
    # plot the binary image obtained with this threshold value here
```

如果运行上述代码片段并可视化在不同阈值下获得的二值图像，您会注意到阈值的选择如何影响最终结果。首先，观察原始图像中像素强度的分布。当使用单个固定阈值时，所有像素被迫分为两类：黑色或白色。

这通常会导致原始图像中平滑过渡的丢失。因此，本应逐渐变化的区域反而呈现为不自然的、锐利的边界或带状图案。这些人为的带状图案被称为*伪轮廓*（它们之所以出现，是因为连续的灰度级被突然分割为仅两个级别，破坏了平滑的强度变化）。简而言之，图像不是平滑的阴影，而是看起来“阶梯状”或“海报化”，这降低了视觉质量，并可能隐藏重要细节。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_314_0.png)

*图 7.7 – 在灰度级别上使用不同阈值创建二值图像*
