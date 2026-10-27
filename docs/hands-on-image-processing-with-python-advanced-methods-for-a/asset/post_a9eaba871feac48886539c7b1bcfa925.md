
To reduce aliasing, the image is resized separately using Lanczos resampling, using the next code snippet. Unlike nearest-neighbor interpolation, `resample=Image.LANCZOS` uses a truncated sinc-based Lanczos filter that combines neighboring pixels to preserve image detail while suppressing high-frequency components responsible for aliasing.

```python
factor, crop_coords = 6, (0, 200, 70, 270)
im_rsz = im.resize((int(im.width/factor), int(im.height/factor)),
    resample=Image.NEAREST, box=crop_coords)
im_rsz_antialiased = im.resize((int(im.width/factor),
    int(im.height/factor)), resample=Image.LANCZOS, box=crop_coords)
# plot im, im_rsz and im_rsz_antialiased here
```

If you run the preceding code snippet, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_198_0.png)

*Figure 4.10 – Downsampling with anti-aliasing using Lanczos with PIL*

As can be seen from the preceding output, the Lanczos resampling method almost got rid of the artifact, whereas the nearest neighbor introduced moiré artifacts.

## Downsampling and anti-aliasing with scikit-image

Direct downsampling often produces aliasing artifacts because high-frequency image details exceed the Nyquist limit and become incorrectly represented at lower resolutions. For example, reducing both image dimensions by a factor of 6 causes one output pixel to represent 6 × 6 = 36 input pixels, making simple pixel sampling insufficient for accurate reconstruction.

Aliasing can be reduced either by increasing the sampling frequency or by suppressing high-frequency components before sampling using low-pass filtering. Anti-aliasing performs this smoothing operation, typically by convolving the image with a Gaussian low-pass filter before downsampling. The next code uses the `skimage.transform.rescale()` function to demonstrate this effect: it rescales the input image UMBC with and without anti-aliasing, and compares the results to observe which one overcomes the aliasing problem.

```python
im = imread('images/umbc.png')
scale, anti_aliasing = 0.3, False # True
im_down = rescale(im, scale=scale, channel_axis=-1,
                  anti_aliasing=anti_aliasing)
```

If you run the above code snippet with a couple of different values of the parameters `scale` and `anti_aliasing`, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_199_0.png)
![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_199_1.png)
![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_199_2.png)
![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_199_3.png)

*Figure 4.11 – Rescaling with and without anti-aliasing using skimage*

Here, `scale=0.3` reduces the image size to 30% of original dimensions, `channel_axis=-1` preserves color channels, and `anti_aliasing=False` disables pre-smoothing, producing stronger aliasing artifacts. Setting `anti_aliasing=True` applies low-pass filtering before rescaling, significantly improving visual quality and reducing distortion in the downsampled image. To learn more about interpolation and anti-aliasing, please visit my blog: https://sandipanweb.wordpress.com/2018/01/21/recursivegraphics-bilinear-interpolation-and-image-transformation-in-Python/.

Now that we have learned how to resize an image (scaling in the domain of the image function), let’s move on to the quantization of an image (which is scaling in the range of the function).

## Quantizing an image

Quantization reduces a large or continuous range of intensity or color values to a smaller discrete set and is therefore related to the number of bits used per pixel. For a digital image $I(x, y)$, quantization can be represented as a mapping function:

$$Q: I(x, y) \to I_q(x, y)$$

where $I_q(x, y)$ denotes the quantized image.

### Uniform quantization

In uniform quantization, the intensity range [$I_{\min}, I_{\max}$] is divided into $K$ equal intervals. The quantization step size is $\Delta = \frac{I_{\max}-I_{\min}}{K}$ and the quantized intensity value is computed as:

$$I_q(x,y)=\left\lfloor\frac{I(x,y)-I_{\min}}{\Delta}\right\rfloor\Delta+\frac{\Delta}{2}$$

where $\lfloor\cdot\rfloor$ denotes the floor operator. Each pixel is mapped to the representative value (typically the midpoint) of its quantization interval. The next code snippet demonstrates uniform quantization of a grayscale image with the function `quantize_image()`:

```python
def quantize_image(img, levels):
    img_min, img_max = img.min(), img.max()
    step = (img_max - img_min) / levels
    quantized = np.floor((img - img_min) / step)*step + step/2
    return np.clip(quantized, 0, 1)

image = img_as_float(rgb2gray(data.astronaut())) # grayscale float in [0,1]
quant_8bit = quantize_image(image, 16) # Quantize to 4-bit (16 levels)
quant_4bit = quantize_image(image, 4) # Quantize to 2-bit (4 levels)
```

If you run the preceding code snippet and visualize the quantized images along with the original image, you should obtain a figure like the following one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_200_0.png)

*Figure 4.12 – Original astronaut image and the 4-bit and 2-bit quantized images*

Here, `step` represents the quantization interval width, `levels` specifies the number of quantization levels, `np.floor()` assigns pixels to discrete intervals, and `np.clip(...,0,1)` constrains intensities to the valid normalized range. The astronaut image is converted to grayscale and normalized to [0,1] using `img_as_float()`.

Quantization to lower bit depths is then performed as `quantize_image()`, where 16 levels simulate 4-bit quantization and 4 levels simulate 2-bit quantization. As the number of quantization levels decreases, smooth gradients and fine image details are progressively lost, illustrating the trade-off between compression and visual fidelity.

### Non-uniform quantization

In non-uniform quantization, intervals are unequally spaced so that more quantization levels are allocated to perceptually important intensity ranges such as shadows or highlights. Methods such as **Lloyd–Max quantization** optimize the interval boundaries using the image histogram.

### Quantizing color images

For RGB images with channels $R$, $G$, and $B$, channel-wise quantization independently quantizes each channel:

$$R_q = Q_R(R),\quad G_q = Q_G(G)\quad\text{and}\quad B_q = Q_B(B)$$

where $Q_R$, $Q_G$, and $Q_B$ are the corresponding quantization functions. Quantization may also be performed in perceptually uniform color spaces such as **Lab**.

### Quantization error and image quality

Quantization introduces approximation error because original intensities are replaced by discrete values.

The quantization error is $e(x,y)=I(x,y)-I_q(x,y)$.

For uniform quantization, the maximum error is bounded by $|e(x,y)|\leq \frac{\Delta}{2}$.

Image quality degradation is commonly measured using mean squared error (MSE):

$$\mathrm{MSE}=\frac{1}{MN}\sum_{x=0}^{M-1}\sum_{y=0}^{N-1}\left(I(x,y)-I_q(x,y)\right)^2$$

where $M\times N$ is the image size. Reducing the number of bits increases quantization error and often introduces contouring, artificial boundaries, and pixelation artifacts.

### Quantization in image compression

Quantization is a fundamental component of lossy compression methods such as JPEG. After transforming the image into the frequency domain using the discrete cosine transform (DCT), the transform coefficients are quantized:

$$C_q(u,v)=\operatorname{round}\left(\frac{C(u,v)}{Q(u,v)}\right)$$

where $C(u,v)$ are the DCT coefficients and $Q(u,v)$ is the quantization matrix. Larger quantization values are typically applied to high-frequency coefficients because they are less perceptually significant, enabling higher compression with limited visual degradation.

### Quantizing a P mode color image with PIL

Color quantization reduces the number of distinct colors in an image by mapping the original colors to a smaller palette, thereby reducing storage requirements while attempting to preserve visual quality. In this example, the PIL `convert()` and `quantize()` methods are used to generate indexed-color (P mode) versions of an image with progressively fewer colors. Image quality is evaluated using the peak signal-to-noise ratio (PSNR), while compressed storage size is measured using `os.path.getsize()`.

The number of quantization levels is generated as: `num_colors_list = [1 << n for n in range(8,0,-1)]`. Here, `1 << n` represents `2^n`, producing the sequence `[256, 128, 64, 32, 16, 8, 4, 2]`. The image is first converted to 8-bit indexed-color mode using an adaptive palette. `convert('P')` converts the image to palette mode, while `Image.ADAPTIVE` selects an optimized palette based on the image content. The image is then quantized to progressively smaller palettes, as shown in the next code snippet:

```python
for num_colors in num_colors_list[1:]:
    im_quant = im.quantize(colors=num_colors)
    # plot im, im_quant side-by-side here
```

Here, `im.quantize(colors=num_colors)` reduces the image to `num_colors` distinct colors. You can use `psnr()` to compute the peak signal-to-noise ratio between the original and quantized images, where larger PSNR values indicate better reconstruction quality, and use `os.path.getsize()` to measure the corresponding storage size in kilobytes. As the number of colors decreases, the image storage size is reduced, but quantization artifacts and loss of visual detail become increasingly noticeable. If you plot quantized images side-by-side, you should obtain the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_202_0.png)

*Figure 4.13 – Image color quantization and compression with PIL*

If you want to understand the impact of color quantization on the SNR of the image (SNR is typically a measure of image quality—the higher the SNR, the better the quality), plot the SNR vs. #colors, and also the image size on disk vs. #colors used in the image (to understand the compression achieved due to color quantization), you should obtain the following figure:
