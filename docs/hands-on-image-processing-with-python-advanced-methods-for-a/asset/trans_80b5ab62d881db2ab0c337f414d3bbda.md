

## 使用3D绘图

接下来的代码使用 Matplotlib 的 plot_surface() 在3D空间中可视化频谱，其中水平轴表示2D频率平面 (u,v)，垂直轴表示由 np.abs() 计算的频谱幅度 (|F(u,v)|)。辅助函数 plot_spectrum_3d() 使用 meshgrid() 创建的坐标网格、颜色映射以及自定义坐标轴格式，为原始频谱和滤波后的频谱生成3D曲面图。生成的图像提供了频率分布的直观视图，并清楚地表明高斯核的傅里叶变换本身也是高斯的，强化了其在频域中作为平滑低通滤波器的作用。现在，请参考以下代码片段：

```
from matplotlib.ticker import LinearLocator, FormatStrFormatter

def plot_spectrum_3d(F, ax, cmap='jet', title=''):
    h, w = F.shape[0], F.shape[1]
    X, Y = range(-h//2, h-h//2), range(-w//2, w-w//2)
    Y, X = np.meshgrid(X, Y)
    surf = ax.plot_surface(X, Y, F, cmap=cmap, linewidth=0, \
            antialiased=False, rstride=5, cstride=5, alpha=0.8)
    ax.set_xlim(-h//2, h//2), ax.set_ylim(-w//2, w//2)
    ax.xaxis.set_major_locator(LinearLocator(5))
    ax.xaxis.set_major_formatter(FormatStrFormatter('%.01f'))
    ax.set_title(title, size=20)
    ax.set_xlabel('u', size=15)
    ax.set_ylabel('v', size=15)
    ax.set_zlabel('F(u,v)', size=15)

fig, axes = plt.subplots(subplot_kw={"projection": "3d"},
                        nrows=2, ncols=2)

axes = axes.ravel()
plot_spectrum_3d( \
    np.abs(fp.fftshift(freq_kernel)), axes[0], title='H(u,v)')
plot_spectrum_3d( \
    np.abs(20*np.log10(0.01+fp.fftshift(freq_kernel))),axes[1])
plot_spectrum_3d( \
    np.abs(20*np.log10(0.01+fp.fftshift(freq))), axes[2])
plot_spectrum_3d( \
    np.abs(20*np.log10(0.01+fp.fftshift(convolved))), axes[3])
```

如果运行上述代码片段，您应该会得到类似下图的图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_255_0.png)

图5.17 – 高斯模糊核在3D中的频谱

## 注意

为什么平移网格？

这会将低频分量居中到中间（正如FFT可视化所预期的那样）。在FFT之前应用 ifftshift 的目的是将低频分量从频谱中心移回角落，这与标准FFT的假设一致，即零频率位于数组原点（左上角）。这种重新定位确保了频域中的乘法正确对应空间域中的卷积，保持了正确的空间对齐，并避免了因频率分量错位而导致的伪影。如果没有这种平移，生成的卷积可能会错误居中或发生环绕，导致错误的滤波结果。

## 应用高斯模糊

现在，我们继续演示高效的频域滤波（其实现得益于卷积定理）。更具体地说，您将看到如何使用几个 Python 库函数，在频域中为几张图像应用高斯模糊滤波。

### 使用 numpy.fft

下面的代码块展示了如何使用卷积定理和 numpy.fft 在频域中应用高斯滤波（因为在频域中它只是简单的乘法）：

```python
def plot_spectrum(F, method=None, cmap='coolwarm'):
    F_disp = (20*np.log10(0.01 + np.abs(F))).astype(int) \\
             if method == 'log' \\
             else np.minimum(np.abs(F).astype(int)+100, 255)
    plt.imshow(F_disp, cmap=cmap, aspect='auto')

im = np.mean(imread('images/lena.jpg'), axis=2)
gauss_kernel = np.outer(signal.windows.gaussian(im.shape[0], 5),
                        signal.windows.gaussian(im.shape[1], 5))
freq = fp.fft2(im)
assert(freq.shape == gauss_kernel.shape)
freq_kernel = fp.fft2(fp.ifftshift(gauss_kernel))
convolved = freq*freq_kernel # by the convolution theorem,
                             # simply multiply in the frequency domain
im_rec = fp.ifft2(convolved).real # imaginary part is artifact
# plot im, im_rec, gauss_kernel;
#      spectrums freq, freq_kernel, convolved
```

> **注意**
> 
> 在应用逆FFT后，我们只取结果的实部，因为虚部通常来自FFT计算过程中的浮点精度误差。理论上，如果所有运算都精确，那么实值图像的逆FFT也应完全是实数。

下图展示了上述代码的输出结果，包括原始*Lena*图像、卷积核以及卷积后得到的输出（同时显示空间域和频域）：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_257_0.png)

*图5.18 – 使用 numpy.fft 在频域中对Lena图像应用高斯模糊*

> **注意**
> 
> FFT，尤其是*Cooley-Tukey算法*，在输入大小为*2的幂*时运行效率最高。这是因为Cooley-Tukey算法将FFT递归拆分为更小的FFT，而2的幂可以实现最大程度的递归拆分。

当输入大小不是2的幂时，像NumPy或OpenCV这样的库通常会在内部对数据进行零填充，将其补到下一个最优长度，以便高效地计算FFT。虽然这提高了速度，但可能会略微影响频率内容和边界行为。

为了获得准确且可重复的结果，尤其是在科学应用中，最好了解并显式控制任何填充行为。如果你在非2的幂大小的图像上运行 `np.fft.fft2()`，并与2的幂版本进行计时比较，你会看到明显的速度差异。

> **提示：**
> 
> 频域滤波假设**周期性边界条件**，这可能在图像边缘产生**环绕伪影**。在FFT之前应用**窗函数**（例如Hann窗或Tukey窗）有助于减少这些伪影。

### 使用 scipy.signal.fftconvolve()

下面的代码块展示了如何使用 scipy.signal 模块的 fftconvolve() 函数在频域中执行卷积（内部仅使用乘法和卷积定理）：

```
im = np.mean(plt.imread('images/mandrill.jpg'), axis=2)
# 2D Gaussian kernel of size 11x11 with σ = 3
gauss_kernel = np.outer(signal.windows.gaussian(11, 3), \
                    signal.windows.gaussian(11, 3))
im_blurred = signal.fftconvolve(im, gauss_kernel, mode='same')
# plot original/blurred image (im, im_blurred) &
# kernel (gauss_kernel)
```

下图展示了运行上述代码块中的图像绘制后得到的输出：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_258_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_258_1.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_258_2.png)

图5.19 – 使用 scipy.signal.fftconvolve 在频域中对山魈图像应用高斯模糊

## 注意：

scipy.signal.fftconvolve() 函数会根据输入大小自动决定是使用直接卷积还是基于FFT的卷积，以选择最快的方法。对于小输入，直接卷积通常更快；对于较大输入，FFT更高效。你可以通过将 method 参数显式设置为 'direct' 或 'fft' 来覆盖此行为。

以下代码片段展示了如何在卷积后绘制原始图像和模糊图像的频谱：

```
F1 = fp.fft2((im).astype(float))
F2 = fp.fftshift( F1 )
# plot the original image spectrum
F1 = fp.fft2((im_blurred).astype(float))
F2 = fp.fftshift(F1)
# plot the blurred image spectrum
```

运行上述代码并绘制图像，你将得到类似下图的输出。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_1.png)

图5.20 – 原始图像和模糊图像的频谱

可以用与前一个示例相同的代码块来计算维多利亚纪念堂图像的模糊版本，但这次调用 fftconvolve() 函数时使用的是更大的（50 × 50）高斯核，σ=10。下图展示了同一代码在重度模糊图像和修改后的核上产生的输出：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_2.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_3.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_4.png)

图5.21 – 在频域中对维多利亚纪念堂图像应用高斯模糊

