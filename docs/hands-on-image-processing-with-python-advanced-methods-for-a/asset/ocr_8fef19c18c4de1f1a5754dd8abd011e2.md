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

# Smoothing with the percentile filter from scipy.ndimage

The `percentile_filter()` function from `scipy.ndimage` generalizes order-statistic filters by selecting a specified percentile from the neighborhood of each pixel. Let \(f(x,y)\) denote the input image and \(N_{x,y}\), a neighborhood of size \(m=(2k+1)^2\). Define the ordered set of intensities in the neighborhood:

\[f_{(1)}, f_{(2)}, \ldots, f_{(m)}, \qquad \text{where } f_{(1)} \le f_{(2)} \le \cdots \le f_{(m)}\]

The percentile filter output is given by:

\[g(x,y)=f_{(r)}, \qquad \text{where } r=\left\lceil\frac{p}{100}\cdot m\right\rceil\]