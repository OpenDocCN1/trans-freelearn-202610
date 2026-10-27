
如果你运行上述代码片段，你应该会得到下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_332_0.png)

图 7.18 – 使用PIL进行颜色增强

## 使用白平衡算法进行颜色校正

白平衡旨在确保场景中的白色在图像中呈现为白色，以补偿光源的色温。以下是一些常用的算法：

- **灰度世界**：假设平均反射率为灰色，即在中性光照下R、G、B的平均强度应相等，并将每个通道缩放，使其平均值与全局平均值相等：$scale_c = \frac{avg_{gray}}{avg_c}$。
- **白平衡（逐通道百分位裁剪）**：独立调整每个R、G、B通道，并裁剪最低和最高百分位（例如0.05%和99.95%）以去除异常值。它将像素值线性重映射到[0, 255]范围：
  
  $$channel = \left(\frac{I - min}{max - min}\right) \times 255,$$
  
  并逐通道去除阴影和高光以归一化亮度。
- **白斑**：假设每个通道中最亮的像素应代表白色，并将每个通道缩放以使其最大强度变为255（$scale_c = \frac{255}{max_c}$）。
- **白斑百分位变体**：与白斑算法思路相同，但使用第95百分位代替绝对最大值。它对于噪声或孤立的镜面高光更加鲁棒。
- **灰度深浅（Minkowski范数）**：通过计算RGB强度的**Minkowski范数**来推广灰度世界算法：
  
  $$Wc = \left(\frac{1}{N}\sum_{i=1}^{N} (I_c(i))^p\right)^{\frac{1}{p}}$$

它在不同光照条件下适应性更强（实践中 $p \approx 6$ 效果良好）。它通过这个估计的光源来归一化每个通道。

下面的代码片段演示了使用上述经典算法进行*白平衡校正*，每种算法都旨在通过调整RGB通道来中和图像的色偏，以模拟理想光照（白光）：

```
# white balance for every channel independently
def white_balance(channel, perc = 0.05):
    mi, ma = (np.percentile(channel, perc), \
              np.percentile(channel,100.0-perc))
    channel = np.uint8(np.clip((channel-mi)*255.0/(ma-mi), 0, 255))
    return channel

image = imread("images/flowers.png")[..., :3]  # load color
balanced  = np.dstack([white_balance(channel, 0.05) \
                       for channel in cv2.split(image)])
# plot image and balanced side-by-side here
def gray_world(img):
    img = img.astype(np.float32)
    avg_rgb = np.mean(img, axis=(0, 1))
    avg_gray = np.mean(avg_rgb)
    scale = avg_gray / avg_rgb
    balanced = img * scale
    balanced = np.clip(balanced, 0, 255).astype(np.uint8)
    return balanced

def white_patch(img):
    img = img.astype(np.float32)
    max_rgb = np.max(img, axis=(0, 1))
    scale = 255 / max_rgb
    balanced = img * scale
    balanced = np.clip(balanced, 0, 255).astype(np.uint8)
    return balanced

def white_patch_percentile(img, percentile=95):
    img = img.astype(np.float32)
    scale = 255 / np.percentile(img, percentile, axis=(0, 1))
    balanced = img * scale
    balanced = np.clip(balanced, 0, 255).astype(np.uint8)
    return balanced
```

```python
def shades_of_gray(img, p=6):
    img = img.astype(np.float32)
    rgb_power = np.power(img, p)
    mean_power = np.mean(rgb_power, axis=(0, 1))
    illuminant = np.power(mean_power, 1/p)
    scale = np.mean(illuminant) / illuminant
    balanced = img * scale
    balanced = np.clip(balanced, 0, 255).astype(np.uint8)
    return balanced

plt.imshow(np.hstack((image, gray_world(image), white_patch(image), \
                     white_patch_percentile(image),
                     shades_of_gray(image))))
```

如果你运行上述代码片段，你应该会得到下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_334_0.png)

*图 7.19 – 使用白平衡算法进行颜色校正*

## 线性噪声平滑

线性（空间）滤波通过计算局部邻域内像素强度的加权和来运作。设$f(x,y)$表示输入图像，$g(x,y)$表示滤波后的输出。线性滤波器的定义如下：

$$g(x,y)=\sum_{i=-k}^{k}\sum_{j=-k}^{k} h(i,j)f(x-i,y-j)$$

其中$h(i,j)$是大小为$(2k+1)\times(2k+1)$的滤波器核（掩码），该操作对应二维离散卷积，可简写为$g=f*h$。线性滤波被广泛用于**噪声抑制（平滑）**，因为它会衰减图像中的高频成分。如果将观测图像建模为：

$$f(x,y)=s(x,y)+n(x,y)$$

其中$s(x,y)$是真实信号，$n(x,y)$是加性噪声，那么线性平滑通过对相邻像素取平均来降低$n(x,y)$的方差。因此，模糊通常被用作*预处理步骤*，在更高层次分析之前去除细小、无关的细节和噪声。

盒式滤波器是最简单的线性平滑算子，其所有权重都相等：

$$h(i,j)=\frac{1}{(2k+1)^2},\qquad \forall(i,j)$$

因此，滤波后的图像变为：

$$g(x,y)=\frac{1}{(2k+1)^2}\sum_{i=-k}^{k}\sum_{j=-k}^{k}f(x-i,y-j)$$

盒式滤波器具有以下效果：

- 每个像素被替换为局部**均值**
- 通过平均降低随机噪声
- 导致**边缘模糊**，因为尖锐的过渡被平滑化

一种更高级的线性滤波器使用高斯核：

$$h(i,j)=\frac{1}{2\pi\sigma^2}\exp\left(-\frac{i^2+j^2}{2\sigma^2}\right)$$

从而得到：

$$g(x,y)=\sum_{i=-k}^{k}\sum_{j=-k}^{k}h(i,j)f(x-i,y-j)$$

这里，权重随距离呈指数衰减，赋予邻近像素更大的重要性，并产生比盒式滤波器更平滑、更自然的模糊效果。

增大核大小$(2k+1)$或高斯标准差$\sigma$：

- 增强平滑效果
- 降低噪声方差
- 但也会移除精细细节

接下来的章节将演示如何对图像应用线性噪声平滑，首先使用`PIL.ImageFilter`类，然后使用`scipy.ndimage`模块的滤波函数。

## 使用盒式模糊核进行平均平滑

使用`PIL.ImageFilter.Kernel()`，盒式滤波器通过离散核实现，例如：

$$h=\frac{1}{9}\begin{bmatrix}1&1&1\\1&1&1\\1&1&1\end{bmatrix}$$

更大的核（例如5×5、7×7）对应于公式中$k$的增大，并产生更强的平滑效果。输出图像通过将该核与噪声图像进行卷积得到：

$$g=f*h$$

随着核大小的增大，噪声被更有效地去除，但边缘和精细结构也会变得越来越模糊。

```python
im = Image.open('images/mandrill_spnoise_0.1.jpg')
for n in [3,5]:
    box_blur_kernel = np.reshape(np.ones(n*n),(n,n)) / (n*n)
    im_smoothed = im.filter(ImageFilter.Kernel((n,n), \
                              box_blur_kernel.flatten()))
    # plot the smoothed image here
```

如果你运行上述代码片段并将图像并排绘制，你应该会得到下图。可以看出，盒式模糊核越大，输出图像被平滑得越厉害。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_336_0.png)

*图 7.20 – 使用不同核大小的盒式模糊滤波器进行去噪*

## 使用高斯模糊滤波器进行平滑

高斯模糊滤波器也是一种线性滤波器，但与简单的均值滤波器不同，它取核窗口内像素的加权平均来平滑某个像素（对应邻居像素的权重随该邻居与中心像素的距离呈指数衰减）。高斯模糊滤波器使用高斯核进行卷积：

g = f * h_σ

其中h_σ是标准差为σ的高斯核

- σ越大 → 核越宽 → 平滑越强
- 噪声逐渐被去除
- 图像细节越发模糊

通过采样二维高斯函数得到5×5的高斯核：

h(i,j) = 1/(2πσ²) exp( - (i² + j²)/(2σ²) ),      i,j ∈ −2,−1,0,1,2

对于常用的σ ≈ 1情况，归一化的5×5核为：

h = 1/273 [1 4 7 4 1; 4 16 26 16 4; 7 26 41 26 7; 4 16 26 16 4; 1 4 7 4 1]

注意

- 该核是对称且可分离的
- 归一化因子（273）确保：Σ_{i,j} h(i,j) = 1
- 中心附近的值较大，赋予邻近像素更高的权重，从而产生平滑、自然的模糊效果。

下面的代码演示了如何使用`PIL.ImageFilter.GaussianBlur()`，通过不同的半径参数值来平滑噪声更大的图像：

```
im = Image.open('images/mandrill_spnoise_0.2.jpg')
for radius in range(1, 4):
    im_smoothed = im.filter(ImageFilter.GaussianBlur(radius))
    # plot the smoothed image here, for the blur-radius of GaussianBlur
```

