

如果运行上述代码并绘制图像，你应该会得到如下图所示的图形：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_490_0.png)

Figure 10.3 – Mathematical morphology to create binary mask from coins image

以下是正在发生的事情的分解：

+   1. **Distance transform:** 使用模块 scipy.ndimage 中的函数 distance_transform_edt() 计算距离变换输出 D（距离），这里峰值对应于硬币中心。
+   2. **Marker extraction:** 使用 skimage.features 模块中的函数 peak_local_max() 在上面计算的距离图上检测局部最大值。这些作为 watershed 的标记。
+   3. **Marker image construction:** 为每个检测到的峰值分配唯一的标签。
+   4. **Watershed segmentation:** 使用 skimage.segmentation 中的函数 watershed() 对 −D(x,y) 应用 watershed 分割，使得峰值成为盆地，watershed 分离接触的硬币。
+   5. **Visualization:** 并排绘制距离变换输出（突出显示中心）、标记图图像（分割的种子）和 watershed 分割（分离的硬币）输出，如下一段代码片段所示：

```
from skimage.feature import peak_local_max
from skimage.segmentation import watershed

distance = ndi.distance_transform_edt(cleaned)
coords = peak_local_max(distance, footprint=np.ones((3, 3)),
labels=cleaned)
markers = np.zeros_like(distance, dtype=int)
for i, (r, c) in enumerate(coords, start=1): markers[r, c] = i
labels = watershed(-distance, markers, mask=cleaned)
# plot the images distance, markers and labels side-by-side here
```

如果运行上述代码并绘制图像，你应该会得到如下图所示的图形：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_491_0.png)

以下是代码中正在发生的事情的分解：

+   1. **Region-based filtering:** 函数 `skimage.measure.regionprops()` 用于计算分割区域的以下属性：面积、周长、偏心率和 solidity。
+   
+   2. **Circularity measure:**
+   
+      $$C = \frac{4\pi A}{P^2},$$
+   
+      其中 $A$ 和 $P$ 分别是相应区域的面积（像素总数）和周长（$C \approx 1 \to$ 圆）。使用它通过阈值过滤非硬币区域，例如 0.7，如代码中所用（硬币应具有比例如具有相同周长的矩形区域更高的圆度值）。它去除噪声和不规则形状。
+   
+   3. **Final counting:** 函数 `skimage.measure.label()` 用于标记精炼区域。然后计算连通分量的数量以找到硬币的数量。
+   
+   4. **Visualization:** 并排绘制 watershed 标签图像、带 regionprop 过滤的 cleaned 标签图像以及带有硬币中心位置标记的叠加图像（使用每个区域的 centroid 属性获得，作为连通分量的几何中心：
+   
+      $$(x_c,y_c)=\left(\frac{1}{A}\Sigma x,\frac{1}{A}\Sigma y\right)$$
+   
+      如下一段代码片段所示：

```
```python
regions = measure.regionprops(labels)
coin_labels = []
for region in regions:
    circularity = 4. * np.pi * region.area / (region.perimeter ** 2 + 1e-6)
    if circularity > 0.7 and region.area > MIN_AREA:
        coin_labels.append(region.label)
coin_mask = np.isin(labels, coin_labels)
clean_labels = measure.label(coin_mask)
num_coins = clean_labels.max()
print(f"Number of coins detected: {num_coins}")
# Number of coins detected: 24
# Plot labels, clean_labels, gray side-by-side and mark centroid here
```
```

虽然 SLIC 基于 solely 颜色和空间邻近性生成超像素，但 MaskSLIC 通过结合先验掩码信息来约束超像素在感兴趣区域内形成，从而扩展了该框架。

- **Mathematical formulation:** SLIC 执行局部的 **k-means clustering**，使用距离度量：
  
  $$D = \sqrt{d_c^2 + \left(\frac{m}{S}\right)^2 d_s^2},$$
  
  其中 $d_c = \sqrt{(L_i - L_j)^2 + (a_i - a_j)^2 + (b_i - b_j)^2}$（颜色距离），$d_s = \sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}$（空间距离），$S$: 预期超像素大小和 $m$: 紧凑度参数。

- **Optimization objective:** SLIC 最小化：
  
  $$\sum_{k=1}^{K} \sum_{i \in S_k} D\left(i, C_k\right)^2$$
  
  其中 $C_k$ 是聚类中心。

- **MaskSLIC extension:** MaskSLIC 引入约束：
  
  $$S_k \subseteq \mathcal{M}$$
  
  其中 $\mathcal{M}$ 是预定义的掩码。关键思想是限制聚类到相关区域，提高边界 adherence 并避免超像素跨越对象边界。

- **Region Adjacency Graph (RAG) merging:** 尽管 *superpixels* 通过对相似像素进行分组显著降低了计算复杂度，但它们通常会产生 *over-segmented* 表示，其中单个语义对象可能被分割成许多小区域。为了获得更大、更有意义的对象级分割， subsequently 使用 Region Adjacency Graph 将外观相似的相邻超像素合并。
  
  超像素化后，图像表示为 **graph over regions** $G = (V, E)$，其中 $V = \{S_1, S_2, ..., S_K\}$ 是超像素集合，$E$ 是 **adjacent regions** 之间的边集合。

- **Node attributes (Region Statistics):** 每个区域 $S_i$ 与特征相关联（例如平均颜色），
  
  $$\mu_i = \frac{1}{|S_i|} \sum_{(x,y) \in S_i} I(x, y),$$
  
  其中 $|S_i|$: 区域中的像素数和 $I(x, y)$: 像素强度（或 RGB/Lab 空间中的向量）。

- **Edge weights (Similarity Measure):** 边编码相邻区域 $w_{ij} = \| \mu_i - \mu_j \|_2$ 之间的 **dissimilarity**，s.t. 小 $w_{ij} \rightarrow$ 相似区域；大 $w_{ij} \rightarrow$ 不相似区域。

- **Hierarchical region merging:** RAG merging 执行 **iterative agglomerative clustering**
  
  $$(S_i, S_j) = \arg \min_{(i,j) \in E} w_{ij}$$
  
  受 $w_{ij} < \tau$ 约束，其中 $\tau$ 是阈值（停止时没有边满足阈值）。

当两个区域 $S_i$ 和 $S_j$ 合并时，$S_{ij}=S_i\cup S_j$，更新均值

$$\mu_{ij}=\frac{|S_i|\mu_i+|S_j|\mu_j}{|S_i|+|S_j|}.$$

合并后，边权重重新计算 $w_{(i,j),k}=\lVert\mu_{ij}-\mu_k\rVert_2$。

这是通过函数 `_weight_mean_color()` 实现的（refer github）。RAG merging 最小化 intra-region variance

$$\min\sum_k\sum_{(x,y)\in S_k}\lVert I(x,y)-\mu_k\rVert^2,$$

受 adjacency constraints 约束。

- **RAG merging transforms:** 超像素 → 语义上有意义的区域，通过逐步对相似相邻区域进行分组，形成低级过分割和高级别分割之间的桥梁。

下一段代码演示并比较 **SLIC** 与 **MaskSLIC**。首先读取输入图像并使用 `rgb2lab()` 转换为 *Lab* 空间以进行 *perceptually uniform* 聚类，同时掩码通过 `remove_small_objects()`、`remove_small_holes()` 和 `opening()` 进行阈值处理和精炼。

函数 `segmentation.slic()` 在 $(x,y,L,a,b)$ 特征空间中生成 SLIC 超像素，而 MaskSLIC 将分割限制在掩码区域内以获得更有意义的超像素。然后对两种分割应用基于 RAG 的合并以获得语义连贯的区域。

辅助函数（`_weight_mean_color()`、`merge_mean_color()`、`merge_segments()`，refer github）使用 `graph.rag_mean_color()` 构造 RAG，并通过 `graph.merge_hierarchical()` 基于平均颜色相似性 *merge adjacent regions* 层次化。

最后，`mark_boundaries()` 并排可视化原始图像、掩码、SLIC、MaskSLIC 及其合并输出。

```
```python
def merge_segments(img, segments):
    rag = graph.rag_mean_color(img, segments)
    labels = graph.merge_hierarchical(
        segments, rag, thresh=35,
        rag_copy=False, in_place_merge=True,
        merge_func=merge_mean_color,
        weight_func=_weight_mean_color)
    return color.label2rgb(labels, img, kind='avg')

img = plt.imread('images/cats.jpg') #input data
lum = color.rgb2lab(img)
mask = color.rgb2gray(plt.imread('images/cat_mask.jpg')) > 0.1
mask = morphology.remove_small_objects(mask, min_size=499)
mask = morphology.remove_small_holes(mask, area_threshold =499)
mask = morphology.opening(mask, morphology.disk(3))
slic = segmentation.slic(lum, n_segments=200, start_label=1)
```

