

- **去模糊化：** 将聚合的模糊集合转换为单个清晰输出（通常通过质心方法）。输入**隶属函数（MFs）**包括以下内容：
- **Dark：** 在强度值的较低范围内定义的梯形 MF
- **Medium：** 以中范围为中心的三角形 MF
- **Bright：** 在较高强度上方的梯形 MF

输入图像的动态范围首先使用最小和最大像素强度确定。然后模糊集合相对于该范围放置。这确保模糊系统适应输入图像的实际强度跨度。输出 MFs 包括以下内容：

- **Darker：** 设计为降低强度
- **Enhanced：** 针对中范围增强
- **Brighter：** 提高强度值

输入到输出的映射由 Mamdani 规则定义。输出也使用 scikit-fuzzy 隶属函数定义（例如，`trapmf`、`trimf`），并且使用相同的论域。

> 模糊系统可以通过增加隶属函数的数量并定义更详细的规则来进一步改进。这允许更精细的控制并更好地适应不同的图像特征。以下实现为可读性而一次处理一个像素。在典型的机器上，512×512 图像预计需要 ~1 分钟。对于生产使用，请考虑使用 `np.vectorize()` 向量化或用 NumPy 数组操作替换循环。

让我们现在考虑以下代码片段：

```
import skfuzzy as fuzz
def fuzzy_enhance_pixel(value):
    dark_level = fuzz.interp_membership(x, input_dark, value)
    medium_level = fuzz.interp_membership(x, input_medium, value)
    bright_level = fuzz.interp_membership(x, input_bright, value)
    rule_darker = np.fmin(dark_level, output_darker)
    rule_enhanced = np.fmin(medium_level, output_enhanced)
    rule_brighter = np.fmin(bright_level, output_brighter)
```

```
```python
aggregated = np.fmax(rule_darker, np.fmax(rule_enhanced,
    rule_brighter))
return fuzz.defuzz(x, aggregated, 'centroid')

def fuzzy_image_enhancement(img_gray):
    img_out = np.zeros_like(img_gray, dtype=np.uint8)
    h, w = img_gray.shape
    for i in range(h):
        for j in range(w):
            pixel = img_gray[i, j]
            enhanced_val = fuzzy_enhance_pixel(pixel)
            img_out[i, j] = np.clip(enhanced_val, 0, 255)
    return img_out

img_path = "images/cheetah.png"
img_gray = cv2.imread(img_path, cv2.IMREAD_GRAYSCALE)
img_min, img_max = int(np.min(img_gray)), int(np.max(img_gray))
x = np.arange(0, 256, 1) # fuzzy membership functions for input / output
range_width = img_max - img_min # fuzzy input MFs key points
low = img_min + 0.25 * range_width
mid = img_min + 0.5 * range_width
high = img_min + 0.75 * range_width
input_dark = fuzz.trapmf(x, [img_min, img_min, low, mid]) # Input fuzzy sets
input_medium = fuzz.trimf(x, [low, mid, high])
input_bright = fuzz.trapmf(x, [mid, high, img_max, img_max])
output_darker = fuzz.trapmf(x, [0, 0, 30, 100]) # Output fuzzy sets
output_enhanced = fuzz.trimf(x, [50, 128, 255])
output_brighter = fuzz.trapmf(x, [150, 200, 255, 255])
# Plot input and output membership functions here, refer to github notebook

img_fuzzy = fuzzy_image_enhancement(img_gray)
# Display result images (img_gray and img_fuzzy)
```
```

如果运行上述代码片段，您应该获得以下图表：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_328_0.png)

*图 7.14 – 模糊隶属函数*

下图显示了使用模糊 HEQ 获得的增强图像，以及并排的原始图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_328_1.png)

*图 7.15 – 模糊直方图均衡化*

让我们现在尝试理解上述代码片段的工作原理：

- **输入/输出 MFs**：根据输入图像范围动态缩放。
- **可视化**：显示 MF 绘图和增强图像输出。
- `fuzzy_enhance_pixel`：使用 `fuzz.interp_membership()` 执行模糊化，通过计算像素值属于每个模糊集合的强度来执行模糊化。对单个像素执行模糊化、推理、聚合和去模糊化。它使用 `np.fmin()` 进行 Mamdani 风格的规则应用（先验和后验之间的最小值）。它使用 `np.fmax()` 聚合规则输出。它使用 `fuzz.defuzz()` 进行基于质心的去模糊化。

- `fuzzy_image_enhancement`：遍历所有像素并应用增强。
- **动态隶属函数**：输入模糊集合根据图像强度范围（img_min、img_max）缩放，使其自适应。
  输出 MF 保持固定，但可以修改以调整对比度增强。
- **映射**：输入到输出的映射是通过预定义的模糊规则实现的。每个输入区域都与特定的变换（更暗、增强或更亮）相关联，通过相应的输出 MF。

模糊对比度增强的优点包括以下内容：

- 自适应于图像特征（最小值/最大值）
- 保留精细细节
- 避免过度增强和伪影
- 人类可理解的规则和推理

## 直方图匹配

**直方图匹配**是一种过程，其中图像被改变，使其直方图与另一个参考（**模板**）图像的直方图相匹配。其用例包括以下内容：

- **遥感**：归一化来自不同传感器的卫星图像
- **医学成像**：标准化在不同条件下拍摄的扫描
- **摄影**：匹配在不同光线或相机下拍摄的照片的美学

算法如下：

1. 如以下图所示，为每个图像计算累积直方图
2. 对于输入图像（待调整）中的任意给定像素值 $x_i$，我们需要在输出图像中找到相应的像素值 $x_j$，通过将输入图像的直方图与模板图像的直方图匹配
3. $x_i$ 像素值具有由 $G(x_i)$ 给出的累积直方图值。找到像素值 $x_j$，使得参考图像中的累积分布值，即 $H(x_j)$，等于 $G(x_i)$。
4. 输入数据值 xi 被 $x_j$ 替换

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_330_0.png)

图 7.16 – 直方图匹配

## 使用 scikit-image

如解释的那样，直方图匹配调整一个图像（源）的颜色分布以匹配另一个图像（参考）的颜色分布，基于它们的直方图。此过程对于校正光照不一致或在图像数据集中实现一致的视觉外观特别有用。

下一个代码片段使用 skimage.exposure 模块中的 match_histograms() 函数。函数 match_histograms() 在 RGB 通道中独立匹配源和参考图像的累积分布函数（CDF）。这里，channel_axis=-1 告诉函数图像数组的最后一个维度表示颜色通道。执行的变换为：

p' = F_r^{-1}(F_s(p))

其中 F_s 和 F_r 分别是源和参考的 CDF，p 是源图像中的像素强度。这确保输出图像中的强度值具有与参考相似的直方图。现在，参考下一个代码片段：

```
from skimage.exposure import match_histograms
# Read source and reference images
source = imread('images/me.png')[..., :3]      # Image to correct
reference = imread('images/lena.jpg')[..., :3]  # Image with desired color profile
# Match histograms across RGB channels
matched = match_histograms(source, reference, channel_axis=-1)
```

```
matched = np.clip(matched, 0, 255).astype(np.uint8)
# plot images (source, reference and matched) here
```

如果运行上述代码片段并绘制图像，您应该获得如下图所示的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_331_0.png)

图 7.17 – 使用 scikit-image 的直方图匹配

skimage.exposure 模块中的函数 histogram() 和 cumulative_distribution() 可用于获取图像的直方图和 CDF（参考 GitHub 笔记本）。

## 颜色增强

颜色增强是数字图像处理中的一个基本步骤，用于改善视觉质量、校正不平衡或产生风格化效果。与操作像素强度的亮度或对比度调整不同，颜色增强侧重于调整图像的饱和度和色度内容。颜色由 RGB 图像中红色、绿色和蓝色通道的组合定义，可以增强或减弱以更好地与人类感知或视觉意图保持一致。增强颜色可以使图像看起来更鲜艳，而降低饱和度可以唤起微妙的艺术情绪。

Python 的 PIL.ImageEnhance 模块为调整颜色饱和度提供了一个简单而强大的接口。以下是演示颜色增强和去饱和的快速示例：

```
img = Image.open("images/parrot.jpg")
enhancer = ImageEnhance.Color(img)
colorful = enhancer.enhance(1.8)
desaturated = enhancer.enhance(0.3)
plt.imshow(np.hstack((img, colorful, desaturated))), plt.axis('off')
```

