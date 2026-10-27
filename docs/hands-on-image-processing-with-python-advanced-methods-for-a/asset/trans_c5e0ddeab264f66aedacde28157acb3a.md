

伪代码中呈现的 Floyd–Steinberg 抖动输出的是二值（黑白）图像，但这段代码更加灵活和强大！该算法的这个版本允许你生成具有 k 个不同灰度级的抖动灰度图像，而不仅仅是 2 个！无论你想要模拟复古打印机还是在保留视觉丰富度的同时压缩图像数据，只需选择：`num_colors = k`（例如，2、4、8...），代码将：

- 自动创建 k 个等间距的灰度级调色板（例如，k=4 时为 [0, 85, 170, 255]）
- 智能地量化并扩散像素误差以产生自然的外观结果
- 显示并验证输出中唯一色调的数量

与通常为二值量化呈现的经典 Floyd–Steinberg 公式不同，这种方法通过选择适当数量的输出强度级（`num_colors=k`）自然地扩展到 k 级灰度量化。调色板随后被构造为均匀间隔的灰度级，并且相同的误差扩散机制无需修改即可应用。该框架可进一步推广到彩色图像，通过对每个通道独立应用抖动处理。

阈值化速度快；它非常适合高对比度应用，如文本。半色调适用于传统印刷，当没有灰度时，而抖动则擅长用最少的灰度级模拟平滑色调。下表比较了上述三种方法：

| Feature | Thresholding | Halftoning | Dithering |
|---|---|---|---|
| Output | Binary | Patterned dots | Multi-tone illusion |
| Complexity | Very low | Medium | High |
| Spatial Context | None | Yes (patterned) | Yes (diffuses error) |
| Visual Quality | Low for photos | Medium–high | High |
| Customization | Static threshold | Grid patterns | Quantization + error |
| Hardware Use | Scanners | Printers | Displays, encoders |

*Table 7.4 – Thresholding vs. Halftoning vs. Dithering*

## 图像增强与直方图处理

直方图处理技术提供了一种更好的方法来改变图像中像素值的动态范围，使得其强度直方图具有期望的形状。正如我们所见，图像增强通过对比度拉伸操作是有限的，因为它只能应用线性缩放函数。

### 直方图滑动（平移）

滑动对每个像素加上或减去一个常数，在保持对比度的同时均匀地移动亮度。数学上可以表示为：

`I' = I + Δ`

下一段代码实现了上述直方图修改方法：

```
def histogram_stretching(image):
    min_val, max_val = np.min(image), np.max(image)
    stretched = 255 * (image - min_val) / (max_val - min_val)
    return stretched.astype(np.uint8)

def histogram_shrinking(image, target_range=(100, 200)):
    min_val, max_val = np.min(image), np.max(image)
    shrunk = (image - min_val) / (max_val - min_val) * \
            (target_range[1] - target_range[0]) + target_range[0]
    return shrunk.astype(np.uint8)

def histogram_sliding(image, shift_value=30):
    slid = np.clip(image + shift_value, 0, 255)
    return slid.astype(np.uint8)

image = cv2.imread('images/mandrill.jpg')
image = cv2.cvtColor(image, cv2.COLOR_BGR2LAB)
stretched_image = image.copy()
shrunk_image = image.copy()
slid_image = image.copy()
i = 0 # only stretch the intensity channel L, the very first channel
stretched_image[...,i] = histogram_stretching(stretched_image[...,i])
shrunk_image[...,i] = histogram_shrinking(shrunk_image[...,i])
slid_image[...,i] = histogram_sliding(slid_image[...,i])
image = cv2.cvtColor(image, cv2.COLOR_LAB2RGB)
stretched_image = cv2.cvtColor(stretched_image, cv2.COLOR_LAB2RGB)
shrunk_image = cv2.cvtColor(shrunk_image, cv2.COLOR_LAB2RGB)
slid_image = cv2.cvtColor(slid_image, cv2.COLOR_LAB2RGB)
plt.imshow(np.hstack((image, stretched_image, shrunk_image, slid_image)))
```

如果你运行上述代码片段，你应该得到以下图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_323_0.png)

Figure 7.11 – Histogram stretching, shrinking, and sliding

### 使用 scikit-image 进行对比度拉伸和直方图均衡化

直方图均衡化使用单调且非线性的映射，该映射重新分配输入图像中的像素强度值，使得输出图像具有均匀的强度分布（平坦的直方图），从而增强图像的对比度。下图描述了直方图均衡化的变换函数：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_323_1.png)

Figure 7.12 – Transform function for histogram equalization

以下代码块展示了如何使用 scikit-image 的 `exposure` 模块的 `equalize_hist()` 函数进行直方图均衡化。直方图均衡化实现有两种不同的风格：一种是对整个图像的全局操作，而第二种是局部（自适应）的，通过将图像分成块并对每个块运行直方图均衡化，使用 `equalize_adapthist()` 函数。下一段代码片段还比较了使用 scikit-image 的两种不同直方图处理技术，即对比度拉伸和直方图均衡化所获得的图像增强效果。

```
im = io.imread('images/beans_g.png')
im_rescale = exposure.rescale_intensity(im, \
    in_range=(0, 100), out_range=(0, 255)) # contrast stretching
im_rescale = im_rescale.astype(np.uint8)
im_eq = exposure.equalize_hist(im) # Global histogram equalization
im_adapteq = exposure.equalize_adapthist(im, \
    clip_limit=0.03) # Local (adaptive) histogram equalization
```

如果你运行上述代码片段并绘制图像以及 CDF（参考 GitHub 笔记本），你应该得到以下图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_324_0.png)

Figure 7.13 – Contrast stretching vs. Local / Global Histogram Equalization

前一幅图显示了前一段代码片段的输出结果。可以看出，自适应直方图均衡化在使输出图像的细节更清晰方面比直方图均衡化提供了更好的结果。

## 基于模糊逻辑的图像增强

我们已经探索了广泛使用的传统图像对比度增强方法，如直方图均衡化（HE）和对比度受限自适应直方图均衡化（CLAHE）。这些方法通常是全局的、缺乏灵活性，或容易受到噪声的影响。模糊逻辑提供了一种强大的替代方案，尤其适用于不确定或不精确的上下文，例如自然场景图像，其中像素强度分类（例如“暗”、“中等”、“亮”）本质上是模糊的。

模糊逻辑通过处理真实度的程度而不是二元决策来模仿人类推理。它通过以下方式工作：

- 定义表示语言类别的模糊集
- 使用隶属函数将输入值与这些模糊集关联
- 创建模糊规则以推断输出

在图像对比度增强中，像素的强度可能不是严格的暗或亮；它可能是部分两者。模糊逻辑通过为不同类别分配隶属度来容纳这一点，从而允许更细致的处理。

### 使用 Mamdani 系统进行模糊逻辑对比度增强

在本节中，让我们探索基于 Mamdani 模糊推理系统（FIS）的模糊逻辑对比度增强。模糊逻辑通过在 0 和 1 之间启用部分真值来模拟人类推理。在图像增强中，像素强度可以通过暗、中等和亮等模糊集来解释。然后模糊规则将这些输入集映射到输出强度校正，例如更暗、增强或更亮。

#### Mamdani FIS

Mamdani 系统是一种广泛使用的模糊推理机制，使用 min-max 操作进行规则评估和聚合，并使用重心去模糊化获得清晰的输出值。Mamdani FIS 包含四个步骤：

- **模糊化：** 将输入（像素强度）转换为隶属度。给定一个像素强度值，模糊化根据隶属函数（MF，例如三角形或高斯）将其映射为隶属度：

  `μ_dark(I), μ_medium(I), μ_bright(I)`

- **规则评估：** 使用模糊规则和逻辑运算（通常为 AND 的 min）来评估结果。应用 min 操作从前件推断输出模糊集：

  `μ_output(z) = max_i [ min( μ_input(I), μ_rule_i(z) ) ]`

  每个规则的触发强度使用 min 运算符确定，最终聚合的模糊输出使用 max 运算符计算。让我们定义以下模糊规则：

  - R₁: IF intensity is dark THEN output is darker
  - R₂: IF intensity is medium THEN output is enhanced
  - R₃: IF intensity is bright THEN output is brighter

- **聚合：** 组合所有规则的输出（通常使用 max）

