

其中 Γ 是搜索范围。我们可以使用 `scipy.optimize.minimize_scalar` 求解该优化问题，最小化负熵。

以下 Python 代码演示了使用 OpenCV、scikit-image 和 SciPy 在灰度图像上实现的整个流程。步骤如下：

- **图像归一化：** 应用伽马变换到 [0, 1] 范围所需的
- **熵函数：** 基于直方图的全局熵计算
- **伽马优化：** 在有界区间 (0.1 到 5.0) 内执行标量函数优化
- **可视化：**
  - 显示原始图像和增强后的图像
  - 使用 2D 滑动窗口（通过 `skimage.filters.rank.entropy`）计算局部熵图，以说明空间对比度变化

现在，参考下一个代码片段：

```python
from skimage.filters.rank import entropy
from skimage.morphology import disk
from scipy.optimize import minimize_scalar

# Load and resize grayscale image
img = cv2.imread('images/moon.jpg', cv2.IMREAD_GRAYSCALE)
img = cv2.resize(img, (256, 256)) # standard size for visualization
# Normalize image to [0, 1]
img_norm = img / 255.0

# Define entropy computation
def compute_entropy(image):
    hist, _ = np.histogram(image, bins=256, range=(0, 1), density=True)
    hist += 1e-12 # to avoid log(0)
    return -np.sum(hist * np.log2(hist))

# Define gamma enhancement
def enhance_with_gamma(image, gamma):
    return np.power(np.clip(image, 0, 1), gamma)

# Objective function for optimization
def objective(gamma):
    enhanced = enhance_with_gamma(img_norm, gamma)
    return -compute_entropy(enhanced) # minimize negative entropy

# Find optimal gamma
res = minimize_scalar(objective, bounds=(0.1, 0.75), method='bounded')
optimal_gamma = res.x
enhanced_img = enhance_with_gamma(img_norm, optimal_gamma)
# Convert to 8-bit image
img_out = (enhanced_img * 255).astype(np.uint8)
entropy_map = entropy(img_out, disk(5))
# Visualization: plot img, img_out and entropy_map here
```

如果运行上述代码并将上述图像并排绘制，您应该得到如下图所示的图形：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_386_0.png)

图 8.7 – 基于熵最大化的伽马校正

以下是观察结果：

- 对于低对比度或暗图像，最优 γ 通常小于 1，图像变亮并增加熵
- 增强是内容自适应的，由图像的固有信息含量驱动
- 局部熵图突出显示具有复杂纹理或边缘的区域

正如上文所述，熵最大化导致 γ 值实现对上述低对比度输入图像的显著对比度增强。

## 使用导数的边缘检测

让我们现在探索依赖于导数的经典边缘检测方法，包括 Sobel、Prewitt、Roberts、Scharr、Canny 和 Laplacian of Gaussian 滤波器。还引入了罗盘和百分位梯度技术用于方向性和局部边缘检测。

### 使用 scikit-image 的 Sobel 边缘检测器

( 一阶 ) 导数可以通过使用有限差分更好地近似。以下图所示的 Sobel 算子被广泛使用：

前一幅图显示了前一代码片段的输出。可以看出，图像的水平和垂直边缘由水平和垂直 Sobel 滤波器检测，而使用 Sobel 滤波器计算的梯度幅值图像检测两个方向上的边缘。

### 使用 scikit-image 的更多边缘检测器

图像处理算法中使用了相当多的不同边缘检测算子；它们都是离散的 ( 一阶或二阶 ) 微分算子，并试图近似图像强度函数的梯度（例如，我们之前讨论过的 Sobel 算子）。以下图表中显示的核是用于边缘检测的几个流行核。例如，近似一阶图像导数的流行导数滤波器是 Sobel、Prewitt、Scharr 和 Roberts 滤波器，而近似二阶导数的导数滤波器是 Laplacian：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_388_0.png)

图 8.10 – Prewitt、Sobel、Scharr、Roberts 和 Laplacian（边缘检测）核

如 scikit-image 文档所述，不同算子计算的梯度的有限差分近似是不同的。例如，Sobel 滤波器通常比 Prewitt 滤波器性能更好，而 Scharr 滤波器比 Sobel 滤波器具有更小的旋转方差。以下代码块将不同的边缘检测器滤波器应用于金门大桥灰度图像并显示获得的梯度幅值：

```python
im = imread('images/goldengate.jpg', as_gray=True) # RGB image to gray scale
edges_rob = filters.roberts(im)
edges_sch = filters.scharr(im)
edges_sob = filters.sobel(im)
edges_pre = filters.prewitt(im)
edges_lap = np.clip(filters.laplace(im), 0, 1)
```

如果运行上述代码并将获得的边缘图像并排绘制，您应该得到如下图所示的图形：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_389_0.png)

图 8.11 - 使用 Prewitt、Sobel、Scharr、Roberts 和 Laplacian 算子检测到的边缘

上图显示了前一代码片段的输出。再次，边缘检测的后处理步骤是非极大值抑制，它会变薄使用一阶导数获得的（较厚）边缘；我们之前没有执行此操作。在下一节中，正如我们将看到的，更先进的 state-of-the-art 边缘检测器 Canny 会自动执行此操作。

### 罗盘边缘检测器

虽然经典边缘检测器如 Sobel 和 Prewitt 估计水平和垂直方向上的强度变化，但罗盘边缘检测器利用多个方向以方向自适应方式更有效地检测边缘。

其中一个罗盘算子是 Kirsch 算子，它在八个罗盘方向（N、NE、E、SE、S、SW、W、NW）计算边缘幅值，在每个像素处突出显示最大方向响应。本章深入探讨 Kirsch 罗盘边缘检测器的理论、数学和 Python 实现。

- `kirsch_edge_detection()`:
  - 将图像转换为灰度
  - 使用 OpenCV 的 `filter2D` 将每个 Kirsch 核应用于图像
  - 将八个生成的滤波图像堆叠成 3D NumPy 数组
  - 使用 `np.max(..., axis=0)` 在各方向上保留每个像素的最大响应
- **最终输出：** 使用最强方向响应突出显示所有方向边缘的边缘图

现在，参考下一个代码片段：

```python
def kirsch_kernels():
    g = [
        np.array([[5, 5, 5], [-3, 0, -3], [-3, -3, -3]]),  # North
        np.array([[-3, 5, 5], [-3, 0, 5], [-3, -3, -3]]),   # NE
        np.array([[-3, -3, 5], [-3, 0, 5], [-3, -3, 5]]),   # East
        np.array([[-3, -3, 5], [-3, 0, 5], [-3, 5, 5]]),    # SE
        np.array([[-3, -3, -3], [-3, 0, -3], [5, 5, 5]]),    # South
        np.array([[-3, -3, -3], [5, 0, -3], [5, 5, -3]]),    # SW
        np.array([[5, -3, -3], [5, 0, -3], [5, -3, -3]]),    # West
        np.array([[5, 5, -3], [5, 0, -3], [-3, -3, -3]])     # NW
    ]
    return g

def kirsch_edge_detection(image):
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    kernels = kirsch_kernels()
    edge_maps = [cv2.filter2D(gray, -1, k) for k in kernels]
    kirsch_response = np.max(np.stack(edge_maps), axis=0)
    return kirsch_response

# Load image
image = cv2.imread("images/goldengate.jpg")
image = cv2.resize(image, (400, 400))
# Detect edges
kirsch_edges = kirsch_edge_detection(image)
# Display image and kirsch_edges here
```

如果运行上述代码，您应该得到如下图所示的图形：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_392_0.png)

*图 8.13 – 检测 Kirsch 罗盘边缘*

罗盘边缘检测器，如 **Kirsch 算子**，由于其对方向性的敏感性，非常有效地进行鲁棒边缘提取。通过考虑八个方向并选择最强响应，它们在边缘方向可变或复杂时优于简单的梯度法。

这种方法在任何需要高保真结构检测的领域都有应用，并作为 Python 图像处理中边缘检测工具包的有价值补充。

### 交互式百分位梯度边缘检测

虽然传统的 Sobel、Prewitt 或 Canny 等方法检测强度急剧变化，但基于秩的滤波器，特别是基于百分位的梯度滤波器，为抗噪边缘检测提供了稳健的替代方案。本节介绍使用 `skimage.filters.rank.gradient_percentile` 函数的基于百分位的梯度计算，并提供由 Gradio 驱动的完全交互式演示。

传统的梯度滤波器使用固定核计算差异，这可能对噪声和强度变化敏感。基于秩的滤波器通过在局部邻域内使用 **秩统计**（如中位数和百分位）来缓解此问题。这使其更稳健于：

- 光照变化
- 异常值和噪声
- 模糊边缘

#### 局部百分位梯度

设 $I(x,y)$ 为灰度图像，$N(x,y)$ 表示像素 $(x,y)$ 周围的邻域。定义：

- $p_0$: 下百分位（例如 10%）
- $p_1$: 上百分位（例如 90%）

则像素 $(x,y)$ 处的 **百分位梯度** 幅值为：

$$G(x,y)=I_{p_1}(x,y)-I_{p_0}(x,y)$$

