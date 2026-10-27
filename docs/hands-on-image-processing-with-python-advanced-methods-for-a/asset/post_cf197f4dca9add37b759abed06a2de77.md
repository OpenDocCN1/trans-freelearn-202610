
- the first exponential encodes spatial proximity (controlled by σₛ)
- σₛ controls the spatial decay (distance in the image plane)
- the second exponential encodes **intensity similarity** (controlled by σᵣ)
- σᵣ controls the **range (intensity) similarity**
- W(x, y) is a normalization factor ensuring the weights sum to 1.

Intuitively, unlike linear filters (e.g., Gaussian blur) that average all nearby pixels, the bilateral filter selectively averages only those neighbors that are both **spatially close and similar in intensity**. As a result, pixels across edges (which have large intensity differences) receive negligible weights, preventing smoothing across boundaries. This allows the filter to **reduce noise in homogeneous regions while preserving sharp edges**, making it particularly effective for natural images where structure and detail must be retained.

Let us now create a noisy image from the following grayscale mountain image. The following code snippet demonstrates how to use the np.random_noise() function:

```python
im = color.rgb2gray(img_as_float(io.imread('images/mountain.png')))
sigma = 0.155
noisy = random_noise(im, var=sigma**2)
```

The next figure shows the noisy image, created by adding random noise to the original image.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_344_0.png)

*Figure 7.26 – Noisy mountain image*

The following code snippet demonstrates how to use the bilateral filter to denoise the previous noisy image, with different values for the parameters, σ_color and σ_spatial. Note that sigma_color in scikit-image corresponds to σᵣ (intensity similarity), and sigma_spatial corresponds to σₛ (spatial proximity).

## Using non-local means

**Non-local means (NLM)** is a nonlinear denoising method that exploits the **self-similarity of image patches.** For an input image $f(x,y)$, the restored value at pixel $(x,y)$ is computed as a weighted average over a (typically large) search window $\Omega$:

```
g(x,y) = 1/Z(x,y) \sum_{(s,t)\in\Omega} w((x,y),(s,t)) f(s,t), \quad Z(x,y) = \sum_{(s,t)\in\Omega} w((x,y),(s,t))
```

where the weights depend on patch similarity:

```
w((x,y),(s,t)) = exp\!\left(-\frac{\|P_{x,y} - P_{s,t}\|_2^2}{h^2}\right)
```

where $P_{x,y}$ and $P_{s,t}$ denote vectorized patches (e.g., $k \times k$ neighborhoods) centered at $(x,y)$ and $(s,t)$, respectively, and $h > 0$ controls the decay of weights.

Instead of averaging only nearby pixels, NLM averages pixels whose surrounding patches look similar, even if they are far apart in the image. As a result, repetitive textures are preserved while noise is suppressed. The parameter $h$ governs selectivity: **small $h$ → only very similar patches contribute (better detail preservation)**, **large $h$ → more patches contribute (stronger smoothing but possible loss of texture)**.

Let’s now demonstrate how to denoise a noisy parrot image using skimage’s implementation of the NLM filter, using the next code snippet. The function denoise_nl_means() from scikit-image performs non-local means denoising by averaging pixels with similar patches; here, patch_size=5 defines 5×5 comparison patches, patch_distance=6 sets the search window (≈13×13), the h parameter controls the decay of weights (smoothing strength), fast_mode=False uses the accurate but slower algorithm, and channel_axis=-1 indicates the image channel dimension.

The h parameter to the function controls the decay in patch weights as a function of the distance between patches. If h is large, it allows more smoothing between dissimilar patches.

```
parrot = img_as_float(imread('images/parrot.jpg'))
sigma = 0.25
noisy = parrot + sigma * np.random.standard_normal(parrot.shape)
noisy = np.clip(noisy, 0, 1)
sigma_est = np.mean(estimate_sigma(noisy, channel_axis=-1))
patch_kw = dict(patch_size=5,     # 5x5 patches
                patch_distance=6, # 13x13 search area
                channel_axis=-1)
```

| Aspect | Bilateral filter | Non-local means (NLM) |
|---|---|---|
| Texture preservation | Limited | Very strong |
| Noise handling | Effective for moderate Gaussian noise | Very effective for Gaussian + structured noise |
| Impulse noise | Less effective | Moderate (median often better) |
| Computation cost | Low to moderate | High |
| Parameters | (\(\sigma_s\)) (spatial), (\(\sigma_r\)) (range) | (h), patch size (k), search window (s) |
| Scalability | Fast, suitable for real-time | Slower; may require optimization |
| Artifacts | Can produce cartoon-like smoothing | Can oversmooth if h is large |
| Best use cases | Edge-preserving smoothing, quick denoising | Texture-rich images, high-quality denoising |
| Implementation complexity | Simple | More complex |
| One-line summary | Fast, local, edge-preserving smoothing | Slow, non-local, texture-preserving denoising |

*Table 7.6 – A comparison of bilateral and NLM filter*

As is evident from the output image, the slow version of the algorithm achieves better PSNR than the faster version, representing a trade-off. Both of the algorithm output images have much higher PSNR than the noisy image.

## Smoothing with the percentile filter from scipy.ndimage

The `percentile_filter()` function from `scipy.ndimage` generalizes order-statistic filters by selecting a specified percentile from the neighborhood of each pixel. Let \(f(x,y)\) denote the input image and \(N_{x,y}\), a neighborhood of size \(m=(2k+1)^2\). Define the ordered set of intensities in the neighborhood:

\[f_{(1)}, f_{(2)}, \ldots, f_{(m)}, \qquad \text{where } f_{(1)} \le f_{(2)} \le \cdots \le f_{(m)}\]

The percentile filter output is given by:

\[g(x,y)=f_{(r)}, \qquad \text{where } r=\left\lceil\frac{p}{100}\cdot m\right\rceil\]

and *p* ∈ [0,100] is the chosen percentile. Special cases include the following:

- *p* = 50 → Median filter: *g(x, y) = median(𝒩ₓ,ᵧ)*
- *p* = 0 → Minimum filter: *g(x, y) = min(𝒩ₓ,ᵧ)*
- *p* = 100 → Maximum filter: *g(x, y) = max(𝒩ₓ,ᵧ)*

The percentile filter selects a value based on the **rank (order)** of intensities, making it robust to outliers. For impulse (salt-and-pepper) noise, extreme values (0 or 1) occupy the lowest or highest ranks and are therefore rejected when *p* ≈ 50. This explains why the median filter (50th percentile) effectively removes such noise while preserving edges better than linear averaging filters.

The following code snippet demonstrates how to use this filter:

```
lena = imread('images/lena.jpg')
noise = np.random.random(lena.shape) # add salt-and-pepper noise to input
lena[noise > 0.9] = 255
lena[noise < 0.1] = 0
for p in range(25, 100, 25):
    for k in range(5, 25, 5):
        filtered = ndimage.percentile_filter(lena, percentile=p,
                                             size=(k,k,1))
        # plot the filtered image here
```

If you run the above code snippet and plot the images, you should obtain the following figure. As can be seen from the figure, lower or higher percentiles tend to bias the output toward darker or brighter values, respectively, whereas the median (*p* = 50) provides a balanced estimate. With a small kernel size, it achieves an effective trade-off between noise **suppression** and detail preservation, making it particularly suitable for images corrupted by impulse noise.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_349_0.png)

*Figure 7.29 – Denoising image with scipy.ndimage’s percentile filter*

## Interactive image enhancement app

In this section, we will build and explain a real-time interactive image enhancement tool using Python’s Streamlit library and a modular enhancement backend. As explained, image enhancement involves improving visual quality or highlighting specific features. Histogram-based methods are widely used for this task. This app supports Global Histogram Equalization (GHE), Brightness Preserving Bi-Histogram Equalization (BBHE), BBHE with Maximum Entropy (BPHEME), Range-Limited Bi-Histogram Equalization (RLBHE), Quantized BBHE (QBHE), and Adaptive Gamma Correction with Color Preservation (AGCCPF).

Each method has specific benefits depending on image content and enhancement goals. To run the app.py successfully, you must install the image-enhancement library, which provides the core image processing algorithms (like GHE, BBHE, etc.) used by the app.

App architecture: The next figure shows a schematic diagram of the basic app architecture:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_350_0.png)

*Figure 7.30 – High-level architecture of the Streamlit application for image enhancement*

The user interacts with the Streamlit UI (app.py) to upload an image, select enhancement methods, and tune parameters. The request is processed by the Image Enhancement Engine (image_enhancement.py) implemented using OpenCV and NumPy, and the enhanced image is returned to the user. This architecture highlights the separation of UI logic (Streamlit) and processing logic (enhancement engine), enabling modularity and ease of extension.
