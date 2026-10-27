
## Frequency-domain formulation

Using the Convolution Theorem, the Fourier transform turns the convolution into multiplication:

$$G(u, v) = F(u, v) \cdot H(u, v) + N(u, v)$$

where $G, F, H, N$ are the Fourier transforms of $g, f, h, n$, respectively.

Thus, if $H(u, v)$ is known, one might attempt to recover $F(u, v)$ via:

$$F(u, v) \approx G(u, v) / H(u, v)$$

This leads to the Inverse Filter, our starting point.

Let us demonstrate inverse filtering using a Gaussian blur model. We start with the grayscale Lena image, apply a Gaussian blur in the frequency domain, and generate a degraded version:

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

Now we divide the blurred spectrum by the blur kernel spectrum. Since division by very small values amplifies noise, we add a small constant $\varepsilon$ for stability:

$$\hat{F}(u, v) = G(u, v) / (H(u, v) + \varepsilon)$$

```
epsilon = 10**-6
freq = fp.fft2(im_blur)
freq_kernel = 1 / (epsilon + freq_kernel) # avoid division by zero
convolved = freq * freq_kernel
im_restored = fp.ifft2(convolved).real
im_restored = im_restored / np.max(im_restored) # Normalize
```

If you run the above code and visualize the input / blurred / restored and difference images obtained, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_1.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_2.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_3.png)

Figure 9.2 – Image restoration with Inverse Filter (with Gaussian blur)

The preceding figures show the frequency spectrum of the inverse kernel (a high-pass filter or HPF), the original Lena image, the blurred Lena image with Gaussian low-pass filter (LPF), and that of the restored image, respectively, in the logarithmic scale. You can use the functions to plot a spectrum (in 2D and 3D) defined in the Fourier transform chapter to obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_4.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_432_5.png)

**The frequency spectrum of the blurred Lena image (with Gaussian LPF)**

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_433_0.png)

**The frequency spectrum of the restored Lena image (with inverse filter)**

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_433_1.png)

Figure 9.3 – Spectrum of Gaussian blur kernel and images with Inverse Filter

Similarly, we can deblur an image blurred with a known motion blur kernel using the inverse filter. The code remains the same; only the kernel changes, as shown in the following code. Note that we need to create a zero-padded kernel of a size equal to the size of the original image before we can apply the convolution in the frequency domain, as shown in the next code snippet:

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

Again, if you plot the images side-by-side, you should get a figure like the following one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_434_0.png)

Figure 9.4 – Image Restoration with Inverse Filter (with motion blur)

As can be seen, the inverse filter is essentially a high-pass filter (HPF), since blurring corresponds to low-pass filtering (LPF).

It works reasonably well for noise-free images with a perfectly known blur kernel. However, when noise is present, inverse filtering fails dramatically because high frequencies (where noise dominates) are amplified. This motivates more robust alternatives, such as the Wiener filter, which balances deblurring against noise suppression.

## Deconvolution with the Wiener filter

In the previous section, we introduced the inverse filter for image deblurring when the blur kernel is known. However, one of its major drawbacks is extreme sensitivity to noise. Even small amounts of noise in the blurred image can cause large instabilities in the reconstructed image.

In real-world imaging systems, noise is inevitable. Thus, an important task in image restoration is not only to undo blurring but also to simultaneously suppress noise. A classical solution to this is the Wiener filter, which formulates image restoration as a statistical estimation problem.

The inverse filter tries to recover $F(u, v)$ by dividing:

$$\hat{F}(u, v) = G(u, v) / H(u, v)$$

This works only when noise $N(u, v) = 0$. But when noise is present, division by small values of $H(u, v)$ amplifies noise drastically, producing poor results.

### Wiener filtering — a statistical solution

The Wiener filter introduces statistical estimation by treating restoration as a minimum mean square error (MMSE) estimation problem:

$$\hat{F}(u, v) = \arg\min_{\hat{F}} E\left[ \left|F(u, v) - \hat{F}(u, v)\right|^2 \right]$$

The closed-form solution for the Wiener filter in the frequency domain is:

$$\hat{F}(u, v) = \frac{H^*(u, v)}{|H(u, v)|^2 + S_\eta(u, v)/S_f(u, v)} G(u, v)$$

where $H^*(u, v)$ is the complex conjugate of $H(u, v)$, $S_\eta(u, v)$ is the power spectral density (PSD) of noise, and $S_f(u, v)$ is the PSD of the original image. The term $S_\eta(u, v)/S_f(u, v)$ is the inverse signal-to-noise ratio (SNR). We need to bear in mind:

- When noise is low, $S_\eta \ll S_f$, the filter behaves like an inverse filter.
- When noise is high, it suppresses noisy frequencies.

Thus, Wiener provides a bias-variance tradeoff between detail preservation and noise suppression. However, in real-world scenarios, the terms $S_\eta(u, v)$ and $S_f(u, v)$ are not known in advance:

- Noise PSD $S_\eta(u, v)$: Requires knowledge of the statistical distribution of sensor or channel noise, which is rarely available.
- Image PSD $S_f(u, v)$: Depends on the content of the original image (textures, edges, etc.), which we do not have access to.

Thus, directly applying the above equation is not feasible for most practical image restoration problems.

### Unsupervised Wiener filtering

The unsupervised Wiener filter addresses this by estimating both $S_\eta(u, v)$ and $S_f(u, v)$ from the observed degraded image itself. The key idea is that, under the degradation model $G(u, v) = H(u, v)F(u, v) + N(u, v)$, we can exploit statistical assumptions about the noise and the structure of the signal to approximate the unknown power spectra. Let's estimate the PSDs. The observed power spectrum is:

$$S_g(u, v) = E\left[|G(u, v)|^2\right] = |H(u, v)|^2 S_f(u, v) + S_\eta(u, v).$$

This provides a direct relationship between the observed degraded image spectrum and the unknown PSDs.

Noise estimation: If the noise is assumed to be white Gaussian noise, then $S_\eta(u, v)$ can be approximated as a constant across frequencies:

$$S_\eta(u, v) \approx \sigma_\eta^2$$

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_437_0.png)

Here are the key points to note:

- The Wiener filter balances deblurring and denoising by adaptively weighting frequencies.
- Unlike the inverse filter, it does not excessively amplify noise at high frequencies.

In practice, Wiener filtering provides visually pleasing restoration when the blur kernel is approximately known, even with moderate noise.

## Tikhonov regularization in image restoration

The inverse filter naively attempts:

$$\hat{F}(u,v)=\frac{G(u,v)}{H(u,v)}$$

The problem is: If $H(u,v) \approx 0$ for some frequencies, division amplifies noise catastrophically. The Wiener filter addressed this by modeling noise and signal power spectra. But what if noise statistics are unknown? This is where regularization approaches, especially *Tikhonov regularization*, come in.

### Variational formulation

Instead of directly inverting, we treat restoration as an optimization problem: find an estimate $f$ that explains the observed data while also respecting some prior belief about natural images. The Tikhonov functional is:

$$\hat{f}=\arg\min_f\left(\|h*f-g\|_2^2+\lambda\|Lf\|_2^2\right)$$

*data fidelity* &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *regularization / prior*

where $\|h*f-g\|_2^2$ ensures the restored image, when blurred, matches the observation (data fidelity), $L f$: a linear operator encoding prior knowledge and $\lambda > 0$: regularization parameter controlling the tradeoff. Common choices for $L$:

- $L=I$: penalizes energy of the image (smooths globally).
- $L=\nabla$: penalizes large gradients, favoring smooth images.

Thus, the prior is often called a fidelity + smoothness prior.
