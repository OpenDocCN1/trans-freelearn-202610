
## With a 3D plot

The next code uses Matplotlib's plot_surface() to visualize frequency spectra in 3D, where the horizontal axes represent the 2D frequency plane (u,v) and the vertical axis represents the spectrum magnitude (|F(u,v)|) computed with np.abs(). The helper function plot_spectrum_3d() generates 3D surface plots for the original and filtered spectra using coordinate grids created with meshgrid(), color mapping, and customized axis formatting. The resulting plots provide an intuitive view of frequency distribution and clearly illustrate that the Fourier transform of a Gaussian kernel is itself Gaussian, reinforcing its role as a smooth low-pass filter in the frequency domain. Now, refer to the following code snippet:

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

If you run the preceding code snippet, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_255_0.png)

Figure 5.17 – Frequency spectrum of the Gaussian blur kernel in 3D

## Note

Why shift the grid?

This centers the low frequencies in the middle (as expected in FFT visualizations). When applying ifftshift before the FFT, the purpose is to move the low-frequency components from the center of the spectrum back to the corners, which aligns with the standard FFT assumption that the zero frequency is at the array origin (top-left corner). This repositioning ensures that the multiplication in the frequency domain corresponds correctly to convolution in the spatial domain, preserving the proper spatial alignment and avoiding artifacts caused by misaligned frequency components. Without this shift, the resulting convolution could be incorrectly centered or wrapped, leading to erroneous filtering results.

## Applying Gaussian blur

Let’s now proceed towards the demonstration of efficient frequency-domain filtering (the implementation is possible because of the convolution theorem). More specifically, you will see how the Gaussian blur filter can be applied to a few images in the frequency domain, using a few Python library functions.

### With numpy.fft

The following code block shows how to apply a Gaussian filter in the frequency domain using the convolution theorem and numpy.fft (since in the frequency domain it is simply multiplication):

```
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
```

> **Note**
> 
> After applying the inverse FFT, we take only the real part of the result because the imaginary part typically arises from floating-point precision errors during FFT computations. In theory, if all operations were exact, the inverse FFT of a real-valued image would also be entirely real.

The following figure shows the output plot of the preceding code, the original *Lena* image, kernel, and output obtained after convolution, both in spatial and frequency domains:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_257_0.png)

*Figure 5.18 – Applying Gaussian blur to Lena image in the frequency domain with numpy.fft*

> **Note**
> 
> The FFT, especially the *Cooley-Tukey algorithm*, operates most efficiently when the size of the input is a *power of two*. This is because the Cooley-Tukey algorithm recursively divides the FFT into smaller FFTs, and powers of two allow maximum recursive splitting.

When the input size is not a power of two, libraries (such as NumPy or OpenCV) often internally zero-pad the data to the next optimal length for efficient FFT computation. While this improves speed, it can slightly affect the frequency content and boundary behavior.

For accurate and reproducible results, especially in scientific applications, it’s good practice to be aware of or explicitly control any padding. If you run `np.fft.fft2()` on an image with a size that’s non-power-of-two and time it versus a power-of-two version, you’ll see noticeable speed differences.

> **Tip:**
> 
> Frequency domain filtering assumes **periodic boundary conditions**, which can cause **wrapping artifacts** at image edges. Applying a **windowing function** (e.g., Hann or Tukey) before the FFT helps reduce these artifacts.

### With scipy.signal.fftconvolve()

The following code block shows how the scipy.signal module's fftconvolve() function can be used to run the convolution in the frequency domain (internally by using just a multiplication and the convolution theorem):

```
im = np.mean(plt.imread('images/mandrill.jpg'), axis=2)
# 2D Gaussian kernel of size 11x11 with σ = 3
gauss_kernel = np.outer(signal.windows.gaussian(11, 3), \
                    signal.windows.gaussian(11, 3))
im_blurred = signal.fftconvolve(im, gauss_kernel, mode='same')
# plot original/blurred image (im, im_blurred) &
# kernel (gauss_kernel)
```

The following figure shows the output obtained by plotting images from the preceding code block:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_258_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_258_1.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_258_2.png)

Figure 5.19 – Applying Gaussian blur to a mandrill in the frequency domain with scipy.signal.fftconvolve

## Note:

The function scipy.signal.fftconvolve() automatically decides whether to use direct convolution or FFT-based convolution depending on the input size, aiming for the fastest method. For small inputs, direct convolution is often faster; for larger inputs, FFT is more efficient. You can override this behavior by explicitly setting the method parameter to 'direct' or 'fft'.

The following code snippet shows how to plot the original and the blurred image spectrum after convolution:

```
F1 = fp.fft2((im).astype(float))
F2 = fp.fftshift( F1 )
# plot the original image spectrum
F1 = fp.fft2((im_blurred).astype(float))
F2 = fp.fftshift(F1)
# plot the blurred image spectrum
```

Plot images obtained by running the preceding code, and you will get something like the following figure.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_1.png)

Figure 5.20 – Frequency spectrum of the original and the blurred image

The same code block as the previous one can be used to compute a blurred version of the Victoria Memorial Hall image, but this time the fftconvolve() function is invoked with a larger (50 × 50) Gaussian kernel with σ=10. The following figure shows the output of the same code with the heavily blurred image and the modified kernel:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_2.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_3.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_259_4.png)

Figure 5.21 – Applying Gaussian blur to the Victoria Memorial Hall image in the frequency domain
