
现在让我们绘制原始图像、应用 `SimilarityTransform` 后得到的图像，以及使用逆变换矩阵应用 `AffineTransform` 后得到的图像。从下图中可以看出，除了裁剪区域（边界外部）之外，原始图像被重建，正如预期的那样：

```python
plot_images([im, im_tformed, im_recon_affine],\
            ['original', 'transformed with SimilarityTransform',\
             'reconstructed with AffineTransform'],\
            'Applying geometric transformations to an image',\
            fontsize=15, figsize=(15,5))
```

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_98_0.png)

*图2.7 – 使用 scikit-image 通过仿射变换重建图像*

## 应用漩涡变换

漩涡变换是一种非线性图像变换，会给图像带来旋转或扭曲的效果。这种变换的特点是围绕指定中心旋转像素，从而产生类似漩涡的图案。漩涡的程度取决于旋转中心和扭曲强度等参数。这种非线性变换由文档定义如下：

考虑输出图像中的坐标 $(x, y)$。漩涡变换的反向映射首先相对于中心 $(x_0, y_0)$ 计算其极坐标 $\rho, \theta$，然后根据以下公式进行变换：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_98_1.png)

*图2.8 – 漩涡变换的公式*

此处，*强度* 是表示漩涡程度的参数，*半径* 表示漩涡的像素范围，*旋转* 增加一个旋转角度。将 *半径* 变换为 *r* 是为了确保变换在指定半径内衰减到约 1/1000<sup>th</sup>。这里，我们将使用 `skimage.transform` 模块中的 `swirl()` 函数。

```python
from skimage.transform import swirl

im_swirled = swirl(im, rotation=0, strength=5, radius=300)

plot_images([im_trans, im_rot, im_rot_center, im_sim, im_swirled], 
    ['translation', 'rotation', 'rotation (centered)', 'similarity transform', 'swirl transform'],
    'geometric transformation with warp()', figsize=(15,5))
```

运行上述代码片段应得到如下图所示的输出：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_99_0.png)

*图2.9 – 使用 scikit-image 的 warp() 进行几何变换*

## 应用分段仿射变换

分段仿射变换将图像划分为较小的区域，并对每个区域应用独立的仿射变换，从而实现灵活的局部扭曲。使用 `skimage.transform` 中的 `PiecewiseAffineTransform` 和 `warp()`，可以从在规则网格上使用 `np.linspace()` 生成的相应源控制点和目标控制点来估计变换，`np.linspace()` 在图像的行和列上创建均匀分布的坐标值。在示例中，通过使用余弦函数修改行坐标来引入正弦波失真，产生波浪状变形，同时保持列坐标不变。使用 `tform.estimate(src, dst)` 估计变换，并使用 `warp(im, tform, output_shape=...)` 应用变换以生成扭曲后的图像。

```python
rows, cols = im.shape[0], im.shape[1]

src_cols = np.linspace(0, cols, 20)
src_rows = np.linspace(0, rows, 10)
src_rows, src_cols = np.meshgrid(src_rows, src_cols)
src = np.dstack([src_cols.flat, src_rows.flat])[0]
```

```
# add sinusoidal oscillation to row coordinates
dst_rows = src[:,1] - np.cos(np.linspace(0, 3*np.pi, src.shape[0]))*50
dst_cols = src[:, 0]
dst_rows *= 1.5
dst_rows -= 1.5 * 50
dst = np.vstack([dst_cols, dst_rows]).T
# assert src.shape == dst.shape, "Source and destination points must have the same shape"
# assert src.shape[0] >= 3, "At least 3 control points are required to estimate the transform"

# Estimate the transformation using PiecewiseAffineTransform
tform = PiecewiseAffineTransform()
ret_val = tform.estimate(src, dst)
# if not ret_val:
# raise ValueError("Estimation failed. Ensure points are valid")

out_rows = im.shape[0] - 1.5 * 50
out_cols = cols

# Apply the transformation to warp the image
out = warp(im, tform, output_shape=(out_rows, out_cols))

plt.imshow(out), plt.axis('off'), plt.title('warped', size=15)
```

运行上述代码片段应得到如下图所示的输出：

扭曲后

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_100_0.png)

图2.10 – 使用 scikit-image 应用分段仿射变换

## 向图像添加随机高斯噪声

随机高斯噪声是一种遵循正态（高斯）分布的统计噪声，通常添加到图像中以模拟真实世界的噪声。

它的特征由**概率密度函数（PDF）** *f* 给出，如下所示：

$$f(x)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

这里，我们有以下内容：

- $x$ 是噪声值
- $\mu$（均值）控制平均亮度偏移
- $\sigma$（标准差）控制噪声的分布范围；$\sigma^2$ 是方差

对于在位置 $(x,y)$ 处像素值为 $I(x,y)$ 的图像，高斯噪声 $N(x,y)\in\mathbb{R}$ 按如下方式添加：

$$I_{noisy}(x,y)=I(x,y)+N(x,y),$$

其中 $I_{noisy}(x,y)$ 是添加噪声后的像素强度，$N(x,y)\sim\mathcal{N}(\mu,\sigma^2)$ 是为每个像素独立采样的高斯噪声。

以下是高斯噪声的影响：

- 低 $\sigma$：噪声微弱，几乎不可见
- 高 $\sigma$：噪声强烈，降低图像质量
- 均值 $\mu\ne 0$：引入图像亮度偏移

我们可以使用 `skimage.util` 模块中的 `random_noise()` 函数向图像添加不同类型的噪声（由 `mode` 输入参数指定）。对于 `mode='gaussian'`（默认模式），均值参数指定均值（$\mu$，默认 0），`var` 指定用于生成噪声的高斯分布的方差（$\sigma^2$）。噪声从 $\mathcal{N}(0,\sigma^2)$ 中抽取，如下所示：

- `var=0.01` $\Rightarrow$ $\sigma=0.1$

> **读者注意**
> 
> 在图像扭曲任务中使用像 `estimate()` 和 `PiecewiseAffineTransform` 这样的变换函数时，有一些重要的注意事项需要了解：
> 
> - `estimate()` 函数不会对无效输入（例如点数组不匹配或控制点少于三个）抛出错误。相反，它会静默失败并返回 `False`，这可能导致 `warp()` 产生意外或未改变的输出。始终检查 `estimate()` 的返回值，并确保提供至少三对非共线点，这是有效局部仿射变换所必需的。
> - 虽然正弦波失真凸显了 `PiecewiseAffineTransform` 的灵活性，但它可能会在三角形边界处引入可见的伪影。这些伪影是由于相邻三角形中局部估计的仿射变换之间的不连续性造成的，并且是这种分段近似方法的一个已知局限性。

- var=0.25 ⇒ σ = 0.5
- 默认情况：var=0.01（即 σ = 0.1）

接下来的代码片段展示了如何向图像添加具有不同方差的（默认）高斯噪声：

```python
from skimage.util import random_noise
from skimage import img_as_float

im = img_as_float(imread("images/parrot.png"))
sigmas = [0.1, 0.25, 0.5, 1]
images, titles = [], []
for i in range(4):
    noisy = random_noise(im, var=sigmas[i]**2)
    images.append(noisy)
    titles.append('Gaussian noise with sigma=' + str(sigmas[i]))
plot_images(images, titles, fontsize=12, figsize=(15,10))
```

运行上述代码片段应得到如下图所示的输出：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_102_0.png)

图2.11 – 向图像添加不同级别的随机正态噪声

## 注意

random_noise() 函数期望输入为 2D 灰度图像（H×W）或 3D 彩色图像（H×W×3）。如果灰度图像被错误地表示为 (H, W, 3)，噪声将独立应用于每个通道，这可能与预期行为不符。

random_noise() 中的 mode 参数指定要添加到输入图像的噪声类型。它决定了所使用的噪声模型。以下是几种可用模式的简要概述（gaussian 是默认模式）：

| 模式 | 描述 |
|------|-------------|
| 'gaussian' | 添加具有零均值和指定方差（var）的高斯噪声 |
| 'poisson' | 根据图像值添加泊松噪声，适用于基于计数的数据 |

