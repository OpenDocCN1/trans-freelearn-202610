

## 频域表述

利用卷积定理，傅里叶变换将卷积转换为乘法：

$$G(u, v) = F(u, v) \cdot H(u, v) + N(u, v)$$

其中 $G, F, H, N$ 分别是 $g, f, h, n$ 的傅里叶变换。

因此，如果 $H(u, v)$ 已知，我们可以尝试通过下式恢复 $F(u, v)$：

$$F(u, v) \approx G(u, v) / H(u, v)$$

这就引出了逆滤波（Inverse Filter），也就是我们的起点。

接下来，我们使用高斯模糊模型来演示逆滤波。我们从灰度 Lena 图像开始，在频域中施加高斯模糊，生成一幅退化版本：

```
im = 255*rgb2gray(imread('images/lena.jpg'))
gauss_kernel = np.outer(signal.gaussian(im.shape[0], 3), # Gaussian blur
                        signal.gaussian(im.shape[1], 3))

freq = fp.fft2(im) # FFT of image and kernel
freq_kernel = fp.fft2(fp.ifftshift(gauss_kernel)) # this is H(u,v)
convolved = freq * freq_kernel  # Apply convolution theorem
im_blur = fp.ifft2(convolved).real
im_blur = 255 * im_blur / np.max(im_blur)  # Normalize
```

现在，我们将模糊图像的频谱除以模糊核的频谱。由于除以非常小的值会放大噪声，我们添加一个小常数 $\varepsilon$ 以保持稳定：

$$\hat{F}(u, v) = G(u, v) / (H(u, v) + \varepsilon)$$

```
epsilon = 10**-6
freq = fp.fft2(im_blur)
freq_kernel = 1 / (epsilon + freq_kernel) # avoid division by zero
convolved = freq * freq_kernel
im_restored = fp.ifft2(convolved).real
im_restored = im_restored / np.max(im_restored) # Normalize
```

如果运行上述代码，并将得到的输入图像、模糊图像、恢复图像和差分图像可视化，你应该会得到类似下图的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_1.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_2.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_3.png)

图 9.2 – 使用逆滤波进行图像恢复（高斯模糊）

上面的图像分别以对数尺度展示了逆滤波核的频谱（高通滤波器，HPF）、原始 Lena 图像、经高斯低通滤波器（LPF）模糊后的 Lena 图像以及恢复后图像的频谱。你可以使用傅里叶变换一章中定义的函数来绘制（2D 和 3D）频谱，得到类似下图的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_4.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_5.png)

**模糊 Lena 图像（经高斯低通滤波）的频谱**

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_433_0.png)

**恢复后 Lena 图像的频谱（经逆滤波）**

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_433_1.png)

图 9.3 – 高斯模糊核频谱与逆滤波图像的频谱

类似地，我们可以使用逆滤波对已知运动模糊核造成的模糊图像进行去模糊。代码保持不变，只有模糊核发生变化，如下面的代码所示。注意，在频域中应用卷积之前，我们需要创建一个与原始图像大小相同的零填充核，如下一个代码片段所示：

```
im = 255 * rgb2gray(imread('images/scene.jpg'))
kernel_size = 21  # create 21x21 horizontal motion blur
mblur_kernel = np.zeros((kernel_size, kernel_size))
mblur_kernel[int((kernel_size-1)/2), :] = np.ones(kernel_size)
mblur_kernel = mblur_kernel / kernel_size
pad_y = (im.shape[0] - kernel_size) // 2 # Zero-pad kernel
pad_x = (im.shape[1] - kernel_size) // 2
mblur_kernel = np.pad(mblur_kernel,
                      ((pad_y, pad_y+1), (pad_x, pad_x+1)), 'constant')
freq = fp.fft2(im) # Apply motion blur via FFT
freq_kernel = fp.fft2(fp.ifftshift(mblur_kernel))
im_blur = fp.ifft2(freq * freq_kernel).real
im_blur /= np.max(im_blur)
```

同样，如果将图像并排绘制，你应该会得到类似下图的结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_434_0.png)

图 9.4 – 使用逆滤波进行图像恢复（运动模糊）

可以看出，逆滤波本质上是一个高通滤波器（HPF），因为模糊对应低通滤波（LPF）。

对于无噪声且模糊核完全已知的图像，逆滤波效果相当不错。然而，当存在噪声时，逆滤波会严重失效，因为高频分量（噪声占主导）被放大了。这就促使人们采用更稳健的替代方案，例如维纳滤波，它在去模糊与噪声抑制之间取得平衡。

## 使用维纳滤波进行反卷积

在上一节中，我们介绍了在模糊核已知的情况下用于图像去模糊的逆滤波。然而，其主要缺点之一是对噪声极其敏感。即使模糊图像中只有少量噪声，也可能导致重建图像出现严重的不稳定。

在真实成像系统中，噪声是不可避免的。因此，图像恢复的一项重要任务不仅是消除模糊，还要同时抑制噪声。一个经典的解决方案是维纳滤波，它将图像恢复表述为一个统计估计问题。

逆滤波试图通过相除来恢复 $F(u, v)$：

$$\hat{F}(u, v) = G(u, v) / H(u, v)$$

这仅在噪声 $N(u, v) = 0$ 时有效。但当存在噪声时，除以 $H(u, v)$ 的小数值会急剧放大噪声，产生糟糕的结果。

### 维纳滤波——一种统计解决方案

维纳滤波引入统计估计，将恢复问题视为最小均方误差（MMSE）估计问题：

$$\hat{F}(u, v) = \arg\min_{\hat{F}} E\left[ \left|F(u, v) - \hat{F}(u, v)\right|^2 \right]$$

维纳滤波在频域的闭式解为：

$$\hat{F}(u, v) = \frac{H^*(u, v)}{|H(u, v)|^2 + S_\eta(u, v)/S_f(u, v)} G(u, v)$$

其中 $H^*(u, v)$ 是 $H(u, v)$ 的复共轭，$S_\eta(u, v)$ 是噪声的功率谱密度（PSD），$S_f(u, v)$ 是原始图像的功率谱密度。$S_\eta(u, v)/S_f(u, v)$ 这项是信噪比（SNR）的倒数。我们需要记住：

- 当噪声较低时，$S_\eta \ll S_f$，该滤波器的行为类似于逆滤波。
- 当噪声较高时，它会抑制有噪频率。

因此，维纳滤波在细节保留与噪声抑制之间提供了偏差-方差权衡。然而，在现实场景中，$S_\eta(u, v)$ 和 $S_f(u, v)$ 事先是未知的：

- 噪声 PSD $S_\eta(u, v)$：需要知道传感器或信道噪声的统计分布，而这很少能获得。
- 图像 PSD $S_f(u, v)$：取决于原始图像的内容（纹理、边缘等），而我们无法获得这些信息。

因此，对大多数实际图像恢复问题而言，直接应用上述方程是不可行的。

### 无监督维纳滤波

无监督维纳滤波通过从观测到的退化图像本身同时估计 $S_\eta(u, v)$ 和 $S_f(u, v)$ 来解决这一问题。其核心思想是：在退化模型 $G(u, v) = H(u, v)F(u, v) + N(u, v)$ 下，我们可以利用关于噪声和信号结构的统计假设来近似未知的功率谱。现在来估计 PSD。观测到的功率谱为：

$$S_g(u, v) = E\left[|G(u, v)|^2\right] = |H(u, v)|^2 S_f(u, v) + S_\eta(u, v).$$

这提供了观测到的退化图像频谱与未知功率谱之间的直接关系。

噪声估计：如果假设噪声为高斯白噪声，则 $S_\eta(u, v)$ 可以由跨频率的常数近似：

$$S_\eta(u, v) \approx \sigma_\eta^2$$

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_437_0.png)

以下是需要注意的要点：

- 维纳滤波通过自适应地对频率加权，从而在去模糊和去噪之间取得平衡。
- 与逆滤波不同，它不会在高频处过度放大噪声。

在实践中，当模糊核近似已知时，即使存在中等程度的噪声，维纳滤波也能提供视觉上令人满意的恢复效果。

## 图像恢复中的吉洪诺夫正则化

逆滤波天真地尝试：

$$\hat{F}(u,v)=\frac{G(u,v)}{H(u,v)}$$

问题在于：如果某些频率处 $H(u,v) \approx 0$，相除会灾难性地放大噪声。维纳滤波通过对噪声和信号功率谱建模解决了这一问题。但如果噪声统计特性未知呢？这时就需要正则化方法，尤其是*吉洪诺夫正则化*。

### 变分表述

我们不直接求逆，而是将恢复问题视为一个优化问题：寻找一个估计 $f$，它既能解释观测数据，又能尊重关于自然图像的某些先验。吉洪诺夫泛函为：

$$\hat{f}=\arg\min_f\left(\|h*f-g\|_2^2+\lambda\|Lf\|_2^2\right)$$

*数据保真项* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *正则化 / 先验*

其中 $\|h*f-g\|_2^2$ 确保恢复图像经过模糊后与观测图像一致（数据保真），$L f$：一个编码先验知识的线性算子，$\lambda > 0$：控制权衡的正则化参数。$L$ 的常见选择有：

- $L=I$：惩罚图像的能量（全局平滑）。
- $L=\nabla$：惩罚大的梯度，促使图像平滑。

因此，这一先验通常被称为“保真 + 平滑”先验。

