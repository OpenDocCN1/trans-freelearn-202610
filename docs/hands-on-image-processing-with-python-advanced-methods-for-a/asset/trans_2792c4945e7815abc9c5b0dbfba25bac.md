

以下是关键步骤：

读取输入图像并进行 **downsample** 以提高效率。使用 `skimage.segmentation` 模块中的函数 `felzenszwalb()` 来实现 **graph-based segmentation**。其内部工作原理如下：

- **1. Building graph:** 图像首先被建模为一个无向图

  $$G=(V,E),$$

  其中 $V$ 表示像素（每个像素是一个节点），$E$ 表示相邻像素之间的边（例如 4- 或 8-连通性）且 $w(i,j)$ 表示边权重，衡量不相似度（例如颜色/强度差异）。

- **2. Formation of components:** 最初，每个像素形成自己的组件：

  $$C_i=\{i\}.$$

  边按 **non-decreasing order of weight** 排序，并迭代合并组件，采用贪心策略（类似于构建最小生成树）。

- **3. Inter-component dissimilarity:** 连接两个组件 $C_1$ 和 $C_2$ 的最小边权重为：

  $$D(C_1,C_2)=\min_{i\in C_1,\,j\in C_2} w(i,j)$$

  可用作区域间相似度的度量。

- **4. Merge criterion:** 若满足以下条件则合并 $C_1$ 和 $C_2$：

  $$D(C_1,C_2)\leq\min(\operatorname{Int}(C_1)+\tau(C_1),\ \operatorname{Int}(C_2)+\tau(C_2)),$$

  其中 $\operatorname{Int}(C)=\max_{e\in MST(C)}w(e)$ 指组件 $C$ 的内部差异（区域内变化），定义为 $C$ 的 **maximum edge weight inside the MST**（最小生成树）的最大值，因此小的 $\operatorname{Int}(C)\rightarrow$ 非常均匀的区域且大的 $\operatorname{Int}(C)\rightarrow$ 区域内变化更大。

  $\tau(C)=\frac{k}{|C|}$ 是一个自适应阈值，$k$ 是控制分割粒度比例参数，$|C|$ 是组件中的像素数。

- **5. Component growth:** 从 **single-pixel components** 开始。如果邻近组件相似（边权重低）且内部变化小，则合并它们。由于 $\tau(C)$，更大的组件需要更强的证据才能合并。组件自适应地从像素生长为区域，确保合并在感知上有意义的边界处停止，从而实现高效且保留边缘的分割。使用函数 `quickshift()` 实现基于模式寻求聚类的分割。它将每个像素视为联合空间-颜色空间 $(x,y,I)$ 中的点，并执行 **kernel density estimation (KDE)**：

  $$p(z)=\sum_i K\left(\frac{\lVert z-z_i\rVert^2}{h^2}\right),$$

  其中 $z=(x,y,I)$：特征向量，$K$：核（例如高斯）和 $h$：带宽（控制邻域大小）。然后每个像素链接到具有更高密度的最近邻，形成一棵树；聚类（段）作为像素收敛到相同的密度模式（局部最大值）而出现。像素“向高密度区域移动”，分组为段，而无需指定聚类数量。

使用函数 `watershed()` 和 `slic()` 分别实现 watershed 分割和 SLIC 超像素化，如下一段代码所示：

```python
from skimage.segmentation import felzenszwalb, slic, quickshift, \
    watershed, mark_boundaries

img = plt.imread('images/rose.jpg')[:, ::2, ::2] # downsamples
segments_fz = felzenszwalb(img, scale=100, sigma=0.5, min_size=50)
segments_slic = slic(img, n_segments=250, compactness=10, sigma=1, \
                    start_label=1)

segments_quick = quickshift(img, kernel_size=3, max_dist=6, ratio=0.5)
gradient = sobel(rgb2gray(img))
segments_watershed = watershed(gradient, markers=250, compactness=0.001)
fz_merged = merge_segments(img, segments_fz)
slic_merged = merge_segments(img, segments_slic)
quick_merged = merge_segments(img, segments_quick)
watershed_merged = merge_segments(img, segments_watershed)
# PLOT the segmented and rag-merged images here
```

如果运行上述代码并绘制图像，你应该得到如下图所示的结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_497_0.png)

Figure 10.7 – Segmentation (Quickshift, Watershed, Felzenszwalb) vs. Superpixelation (SLIC)

超像素化不是最终分割，它充当中间层表示。它能够在深度网络中实现高效的基于图的 learning 和区域池化。基于超像素的分割提供了一种强大的抽象：SLIC 在 5D 特征空间中聚类像素。MaskSLIC 结合先验掩码以提高精度。多种算法提供权衡。SLIC 与 RAG 结合：像素 → 超像素 → 区域。这种分层视图在经典分割和现代深度学习 pipeline 之间形成关键桥梁，同时实现效率和可解释性。

## Variational and energy-based segmentation

变分方法将图像分割表述为 **energy minimization problem**，目标是找到一个最优划分（或表示），在 **data fidelity** 和 **regularity** 之间取得平衡：

$$E = E_{\text{data}} + \lambda E_{\text{regularization}},$$

其中 $E_{\text{data}}$：确保与观测图像相似，$E_{\text{regularization}}$：强制平滑或结构约束且 $\lambda$：权衡参数。这些方法深深植根于 **calculus of variations**，导致 **Euler–Lagrange PDEs** 和迭代优化。

### Chan–Vese active contours

Chan–Vese 模型是一种 **region-based active contour method**，**does not rely on image gradients**，使其对弱或缺失边缘具有鲁棒性。

- **Energy functional:**

  $$E(c_1, c_2, C) = \mu \cdot \text{Length}(C) + \lambda_1 \int_{\text{inside}(C)} |I - c_1|^2 dx + \lambda_2 \int_{\text{outside}(C)} |I - c_2|^2 dx,$$

  其中 $C$：轮廓（分割边界），$c_1, c_2$：内部/外部均值强度，$\mu$：平滑权重，$\lambda_1, \lambda_2$：数据保真权重。

- **Level set formulation:** 轮廓 $C$ 使用 **level set function** $\phi(x, y)$ 隐式表示：

  $$C = \{(x, y) \mid \phi(x, y) = 0\}$$

  使用 Heaviside 函数 $H(\phi)$，能量变为：

  $$E(\phi) = \mu \int |\nabla H(\phi)| dx + \lambda_1 \int |I - c_1|^2 H(\phi) dx + \lambda_2 \int |I - c_2|^2 (1 - H(\phi)) dx$$

- **Euler–Lagrange evolution:** 最小化导致以下梯度更新，

  $$\frac{\partial \phi}{\partial t} = \delta(\phi) \left[ \mu \text{div}\left(\frac{\nabla \phi}{|\nabla \phi|}\right) - \lambda_1 (I - c_1)^2 + \lambda_2 (I - c_2)^2 \right]$$

下一段代码实现 Chan–Vese 分割：使用 `skimage.segmentation` 模块中的函数 `chan_vese()`。关键参数是 `mu`（平滑，控制轮廓规则性），`lambda1`、`lambda2`（区域拟合权重），`init_level_set`（初始轮廓，这里为棋盘格）和 `extended_output`（函数返回分割掩码，并且当 `extended_output=True` 时可选地返回水平集函数和能量历史）。如下一段代码所示，它返回一个包含 3 个元素的元组：分割（`cv[0]`：最终二值掩码，1 → 区域内部，0 → 区域外部），水平集函数（`cv[1]`：最终隐式轮廓 $\phi(x, y)$，零水平定义边界）和能量演化（`cv[2]`：迭代过程中的能量值列表，用于监控收敛）。

```python
from skimage.segmentation import chan_vese
image = img_as_float(data.camera())
cv = chan_vese(image, mu=0.25, lambda1=1, lambda2=1,  # play with parameters
    tol=1e-3, max_num_iter=200, dt=0.5,  # to see how they impact the result
    init_level_set="checkerboard", extended_output=True)
# Plot image, cv[0], cv[1], cv[2] side-by-side
```

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_499_0.png)

*Figure 10.8 – Segmentation with Chan-Vese (active contour)*

轮廓演化以最小化能量，基于强度统计分离区域。上述代码实现 **region-based segmentation** 而不依赖边缘。即使 **edges are weak or missing** 也能工作。分割由 **region homogeneity** 驱动。

### Mumford–Shah functional

Mumford–Shah 模型是分割和边缘检测的 **foundational variational framework**。

- **Functional definition:**

  $$E(u,\Gamma)=\int_{\Omega}|I-u|^2dx+\lambda\int_{\Omega\setminus\Gamma}|\nabla u|^2dx+\nu|\Gamma|,$$

  其中 $u(x)$：图像的分段光滑近似，$\Gamma$：边缘集（不连续性）且 $|\Gamma|$：边缘长度。这里，**Data term**：对图像的保真度（第 1 项），**Smoothness term**：分段光滑区域（第 2 项）且 **Edge term**：惩罚边界复杂性（第 3 项）。算法平衡 **denoising + segmentation + edge detection**。

- **Ambrosio–Tortorelli approximation:** 直接最小化困难，因此通过引入辅助变量 $v(x)$ 进行近似：

  $$E(u,v)=\int\left[\beta(u-f)^2+\alpha v^2|\nabla u|^2+\frac{1}{2}\left(\rho|\nabla v|^2+\frac{(1-v)^2}{\rho}\right)\right]dx,$$

  其中 $v(x)\approx 0$ 在边缘处，$v(x)\approx 1$ 在光滑区域且 $\rho\to 0$：尖锐边缘近似。

- **Euler–Lagrange PDEs:**

  $$\frac{\partial u}{\partial t}=\nabla\cdot(v^2\nabla u)-\frac{\beta}{\alpha}(u-f),\qquad \frac{\partial v}{\partial t}=\nabla^2v-\frac{2\alpha|\nabla u|^2v}{\rho}-\frac{v-1}{\rho^2}$$

