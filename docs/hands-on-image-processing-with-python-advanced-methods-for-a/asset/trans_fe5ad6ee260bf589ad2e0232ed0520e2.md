

文件 app.py 作为您使用 Streamlit 构建的交互式图像增强工具的主应用程序脚本。它将用户输入（通过 Streamlit）和后端处理（通过 image_enhancement 模块）结合起来，为交互式尝试不同的图像增强技术构建响应迅速且直观的 GUI。以下是它的简要说明。

Python 脚本 app.py 创建了一个图形化 Web 界面，允许用户：

- 上传一张图像
- 从列表中选择图像增强方法
- 可选地调整方法特定的参数（如分箱数或 alpha）
- 查看原始图像与增强图像之间的实时视觉对比

下表总结了关键函数，如下一段代码所示：

| Section | Functionality |
|---|---|
| `st.file_uploader(...)` | 允许用户上传图像 |
| `Image.open(...), cv2.cvtColor()` | 读取并转换图像以进行处理 |
| `image_enhancement.IE(...)` | 初始化图像增强引擎（来自单独的 Python 模块） |
| `st.selectbox(...)` | 允许用户选择增强算法（启用模型选择） |
| `st.slider(...)` | 允许用户为所选方法动态调整参数 |
| Conditional if-elif block | 将所选增强方法应用于上传的图像 |
| `cv2.cvtColor(...)` again | 将结果从 BGR 转换为 RGB，以便在 Streamlit 中正确显示 |
| `st.columns(...) + st.image(...)` | 并排显示原始图像和增强图像 |

*Table 7.7 – Summary of the key functions in app.py*

其工作原理如下（高层流程）：

1. 用户上传图像
2. 用户选择增强方法
3. 可选滑块出现（如果方法需要参数）
4. 应用使用所选增强方法处理图像
5. 原始图像和增强图像并排显示

现在，参考下面 app.py 的代码：

```
## app.py
import streamlit as st
from image_enhancement import image_enhancement

st.title("📸 Selective Image Enhancement with Streamlit")
uploaded = st.file_uploader("Upload an Image", type=['png', 'jpg', 'jpeg'])
if not uploaded:  st.stop()
img = np.array(Image.open(uploaded).convert('RGB'))  # PIL to OpenCV BGR
bgr = cv2.cvtColor(img, cv2.COLOR_RGB2BGR)
ie = image_enhancement.IE(bgr, color_space='HSV')
method = st.selectbox("Enhancement Method", ["Global HE (GHE)", "Brightness Preserving HE (BBHE)"])  # refer git to more methods
bins, alpha = None, None
if method == "Quantized BBHE (QBHE)":
    bins = st.slider("Number of Gray Levels (QBHE)", \
        min_value=2, max_value=256, value=128)
elif method == "Adaptive Gamma ColorPreserving (AGCCPF)":
    alpha = st.slider("Alpha (AGCCPF)", min_value=0.0, \
        max_value=5.0, value=1.0)
if method == "Global HE (GHE)": out = ie.GHE()  # refer git to more methods
elif method == "Brightness Preserving HE (BBHE)": out = ie.BBHE()
enhanced = cv2.cvtColor(out, cv2.COLOR_BGR2RGB)
col1, col2 = st.columns(2)
with col1: st.image(img, caption="Original Image", use_column_width=True)
with col2: st.image(enhanced, caption="Enhanced Image", use_column_width=True)
```

从 Jupyter Notebook 单元格内部或从控制台运行以下命令以运行应用程序：

```
! python -m streamlit run app.py
```

它默认在 localhost:8501 打开一个浏览器窗口（或指定的端口）。现在，上传一张图像并交互式尝试不同的增强效果。下一张截图显示了应用程序的外观：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_353_0.png)

*Figure 7.31 – Streamlit app for image enhancement*

主要优势包括以下内容：

- **Modular Design:** 可轻松扩展以添加新的增强方法
- **Streamlit Integration:** 快速部署，支持滑块、文件上传和图像显示
- **Interactive UI:** 参数调整的即时反馈
- **Color Space Flexibility:** 可适应不同类型的视觉输入（HSV、YCrCb 等）

强烈建议读者通过集成以下附加功能来增强此应用程序的功能：

- 支持更多增强算法（例如，CLAHE、Retinex、去雾）
- 直方图实时可视化
- 批量图像处理
- 增强输出的下载按钮
- 性能指标，如熵或对比度评分

该应用程序的模块化架构使其易于扩展和定制。将此应用程序视为构建更强大、更具交互性的图像增强工具的基础！

## Denoising with BM3D

图像去噪是图像处理中的一项基本任务，旨在从噪声观测中恢复原始图像。**Block-Matching and 3D (BM3D)** 滤波，由 Dabov 等人于 2007 年提出，是一种最先进的去噪算法，以其在有效去除噪声的同时保留边缘和纹理的能力而闻名。本节全面解释了 BM3D，包括其理论、数学公式和 Python 实现。

设噪声图像 $y$ 被建模为 $y = x + n$，其中 $x$ 是原始干净图像，$n$ 是加性高斯白噪声（AWGN），其标准差为（有关深入讨论，请参阅图像恢复章节）。

BM3D 在两个主要阶段运行：

- **Basic estimate**（硬阈值）：分组相似的块，执行协作 3D 滤波，通过变换域中的硬阈值
- **Final estimate**（维纳滤波）：使用基本估计执行维纳滤波以获得更好的精度

总体 BM3D 去噪流程由三个基本操作组成，即块匹配、协作滤波和聚合，这些操作在上述两个估计阶段中迭代执行：

- **Block matching:** 在图像中查找相似的 2D 块 {$P_i$}。这些块堆叠成 3D 数组：[$P_i^1$, $P_i^2$, ..., $P_i^k$]。
- **Collaborative filtering:** 应用 3D 线性变换（例如，2D DCT + 1D Haar）。执行阈值或维纳滤波。逆变换得到去噪块。
- **Aggregation:** 重叠块使用加权平均聚合以形成最终图像。

要在 Python 中使用 BM3D，让我们首先按下一段代码所示安装 bm3d 库：

```
import bm3d  # first install with !pip install bm3d
image_path = 'images/mandrill_spnoise_0.3.jpg'  # Load a noisy image
noisy_image = img_as_float(io.imread(image_path))
denoised = bm3d.bm3d(noisy_image, sigma_psd=0.2, \
                    stage_arg=bm3d.BM3DStages.ALL_STAGES)
# Display the results here
```

如果您运行上述代码片段并绘制输入和输出图像，您应该得到以下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_355_0.png)

*Figure 7.32 – Denoising with BM3D*

前面的代码片段演示了如何评估和可视化 BM3D 去噪算法在不同噪声标准差（sigma_psd）设置下的性能，使用标准图像质量指标，如 **PSNR** 和 **Structural Similarity Index Measure (SSIM)**。以下是关键步骤：

- 向干净的灰度图像添加合成噪声
- 使用一系列 sigma_psd 值用 BM3D 进行去噪
- 评估去噪后的图像质量
- 可视化结果；原始、噪声和多个去噪输出显示在子图网格中，并附带其关联的 σ 值

**Core functions:**

- `bm3d.bm3d(...)`: 应用完整的 BM3D 去噪流程
- `peak_signal_noise_ratio(...)`: 衡量去噪图像与原始图像的接近程度（越高越好）
- `structural_similarity(...)`: 评估结构相似性（接近 1 的值更好）
- `plt.imshow(...)`: 显示每个结果以便比较

现在，向灰度 cameraman 图像（来自 `skimage.data`）添加高斯噪声，并使用不同的 sigma_psd 值应用 BM3D 去噪，并将您的 PSNR 和 SSIM 结果与 GitHub 笔记本中的我的参考实现进行比较。您应该得到如下图所示：

