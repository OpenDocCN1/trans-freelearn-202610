

如果运行上述代码，绘制图像/直方图，你应该会得到如下图所示的图形：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_485_0.png)

*Figure 10.1 – Thresholding with multi-Otsu*

多阈值 Otsu 是一种阈值分割算法，用于根据图像中灰度级的强度将输入图像的像素分离为若干个不同的类别。它是一种**简单而强大**的无监督分割方法，其可解释性建立在统计方差之上。可以将其视为经典图像处理与现代基于学习的分割之间的桥梁。

多阈值 Otsu 假设**强度可分离性**。当直方图具有明显的峰值且噪声较低时效果最佳。对光照变化和重叠的强度分布敏感。

> **Note**
>
> 尽管有更先进的分割技术可用，阈值分割仍然被广泛使用，因为其简单、计算高效，并且在强度分离清晰的场景中性能强劲，例如生物医学成像和文档分析，在这些场景中它通常作为快速可靠的基线。

## 边缘、区域和地形分割

分割技术大致可分为**基于边缘**、**基于区域**和**地形（形态学）**方法。基于边缘的方法依赖于检测强度不连续性，基于区域的方法利用同质性，而地形方法将图像解释为几何表面。本节重点介绍**分水岭分割**作为统一的地形方法，随后介绍结合区域和形态学进行物体计数的实用流程。

### Watershed segmentation

分水岭分割是一种强大的形态学方法，其根源于**数学形态学**（由 Jean Serra 和 Georges Matheron 创立）。它将图像解释为**地形表面**，其中像素强度 → 海拔，暗区域 → 山谷（盆地），亮区域 → 山峰（峰顶）。

设 $I(x, y)$ 为输入图像。梯度幅值：

$$\nabla I = \sqrt{\left(\frac{\partial I}{\partial x}\right)^2 + \left(\frac{\partial I}{\partial y}\right)^2}$$

表示**边缘强度**，形成地形表面。

分水岭分割定义为：

$$L = \text{Watershed}(\nabla I, \text{markers}),$$

其中 $\nabla I$ 是梯度图像（地形表面），markers 是指示区域的初始种子，$L$ 是标记的分割图。

### Flooding analogy

想象从标记点开始的水漫过程：

- 1. 水从标记开始填充盆地。
- 2. 随着水位上升，盆地扩张。
- 3. 当两个盆地相遇时，会形成分水岭线（边界）。

该过程产生的分割边界与**高梯度区域（边缘）**对齐。

### Marker-based watershed

为避免过度分割，使用**标记控制分水岭**：

- 内部标记 → 物体区域
- 外部标记 → 背景

标记定义为：

$$M(x, y) = \begin{cases} k, & \text{if pixel belongs to marker } k \\ 0, & \text{otherwise} \end{cases}$$

下面的代码片段演示了使用分水岭的灰度图像分割。以下是关键步骤：

- **1. Image loading and preprocessing:** 函数 `rgb2gray()` 来自 `skimage.color` 模块将 RGB 图像转换为灰度，确保基于强度的处理。
- **2. Noise reduction:** 函数 `median()` 来自 `skimage.filters.rank` 模块使用半径为 2 的 **disk** 结构元素（SE）应用**中值滤波**并在保留边缘的同时去除噪声。数学上：

$$I'(x, y) = \text{median}\{I(s, t) \mid (s, t) \in \mathcal{N}(x, y)\},$$

其中 $\mathcal{N}$ 指像素 $(x, y)$ 周围的邻域。

- **3. Marker extraction:** 首先计算局部梯度：

$$G(x,y) = \max_{(s,t)\in \mathcal{N}(x,y)} I(s,t) - \min_{(s,t)\in \mathcal{N}(x,y)} I(s,t)$$

（衡量像素周围**局部**的**强度变化**程度），随后进行阈值化，将**平坦区域（低梯度）**识别为标记。接下来使用 `scipy.ndimage` 模块中的 `label()` 函数进行连通分量标记，分配唯一 ID。

- **4. Gradient computation:** 函数 `rank.gradient()` 产生**地形表面**。薄边缘（小磁盘）确保边界精确。
- **5. Watershed transformation:** 使用 `skimage.segmentation` 模块中的 `watershed()` 函数实现。从标记开始淹没。边界在高梯度脊线处形成。
- **6. Visualization:** 原始图像、梯度图、标记图和最终分割叠加图像使用 matplotlib 并排绘制，如下一段代码所示：

```
from scipy import ndimage as ndi
from skimage.filters import rank
from skimage.util import img_as_ubyte

image = img_as_ubyte(rgb2gray(imread('images/earth.png')[..., :3]))
denoised = rank.median(image, disk(2))
markers = rank.gradient(denoised, disk(5)) < 20 # threshold
markers = ndi.label(markers)[0]
gradient = rank.gradient(denoised, disk(2))
labels = watershed(gradient, markers)
# display image, gradient, markers and labels overlayed side-by-side here
```

如果运行上述代码并绘制图像，你应该会得到如下图所示的图形：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_487_0.png)

*Figure 10.2 – Segmentation with marker-based watershed*

> **Note**
>
> `img_as_ubyte()` 函数将 `rgb2gray()` 产生的归一化浮点图像 `[0,1]` 转换为 8 位（`uint8`）图像 `[0,255]`，这是 `skimage.filters.rank.median()` 的预期输入格式。
>
> **Advantages:** 精确的边界定位，并且自然地结合了边缘信息。
>
> **Limitations:** 对噪声敏感 → 过度分割，需要良好的标记选择。

从关注使用由精心定义的标记引导的梯度驱动淹没来分离区域的基于标记的分水岭分割出发，我们现在转向一个实际应用，在该应用中分割与形态学操作相结合以解决一个现实世界的视觉任务——使用分割和形态学计数硬币。

## Counting coins with segmentation and morphology

### Problem formulation

给定包含多个硬币的灰度或 RGB 图像 $I(x,y)$，我们希望确定**单个硬币的数量** $N_c$。我们可以用 $N_c = \max(L)$ 来估计硬币数量，其中 $L$ 是标记的分割图。这可以形式化为以下流程：

- **1. Thresholding/Binary segmentation:** 将前景（硬币）与背景分离。
- **2. Morphological processing/Cleaning** (opening, closing, distance transform, watershed): 清理掩膜，细化分割，并在必要时分离接触的硬币。
- **3. Distance transform:** 计算每个前景像素到最近背景像素的距离。数学上，

$$D(x,y)=\min_{(x',y')\in\mathrm{background}}\lVert(x,y)-(x',y')\rVert$$

- **4. Watershed separation:** 使用从标记开始的地形淹没来分割接触的物体。
- **5. Connected component labeling:** 为每个连通的硬币分配唯一标签，识别每个硬币。
- **6. Counting:** 计算唯一标签的数量。
- **7. Binary segmentation:** 阈值化从灰度图像 $I$ 产生掩膜 $M$：

$$M(x,y)=\begin{cases}1, & I(x,y)>T\\0, & I(x,y)\le T\end{cases}$$

其中 $T$ 是阈值，使用 Otsu 方法计算。

### Morphological operations

形态学操作有助于**清理二值掩膜并分离接触的硬币**。设 $B$ 为二值图像，$S$ 为 SE（结构元素，一种用于在形态学操作中探测和处理图像邻域的小型预定义形状/核）。现在，让我们定义形态学操作：

- **Erosion** ($\ominus$): $B \ominus S = \{z \mid S_z \subseteq B\}$。去除小的噪声像素并缩小物体。
- **Dilation** ($\oplus$): $B \oplus S = \{z \mid S_z \cap B \neq \varnothing\}$。扩张物体并连接附近像素。
- **Opening** ($\circ$): $B \circ S = (B \ominus S) \oplus S$。去除小物体/噪声并平滑边界。
- **Closing** ($\bullet$): $B \bullet S = (B \oplus S) \ominus S$。填充物体内部的小孔。
- **Distance transform and watershed:** 为分离接触的硬币，首先计算二值掩膜的距离变换 $D(x,y)$，其中峰值对应硬币中心。将其用作标记并应用**分水岭算法**以将重叠区域分割为不同的独立物体。
- **Labeling and counting:** 分割和形态学细化后，二值掩膜包含**连通分量**。标记每个分量：$L = \operatorname{label}(M)$。**硬币数量**为：$N_c = \max(L)$。

让我们使用以下代码片段实现硬币计数。以下是关键步骤：

- **1. Load image:** 使用 `skimage.data` 模块中的 `coins()` 函数加载硬币图像。
- **2. Thresholding:** 使用 `skimage` 中的 `filters.threshold_otsu()` 函数将前景硬币与背景分离。
- **3. Morphological cleaning:** 使用 `skimage` 中的 `morphology.remove_small_objects()` 函数和 `scipy.ndimage` 中的 `binary_fill_holes()` 函数分别去除噪声并填充硬币内部的孔洞。

```python
from skimage import io, color, filters, measure, morphology, data
gray = data.coins()
thresh = filters.threshold_otsu(gray)
binary = gray > thresh
cleaned = morphology.remove_small_objects(binary, min_size=100)
cleaned = ndi.binary_fill_holes(cleaned)
# plot gray, binary and cleaned images side-by-side here.
```

