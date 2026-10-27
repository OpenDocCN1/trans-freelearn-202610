+   - the first exponential encodes spatial proximity (controlled by σₛ)
+   - σₛ controls the spatial decay (distance in the image plane)
+   - the second exponential encodes **intensity similarity** (controlled by σᵣ)
+   - σᵣ controls the **range (intensity) similarity**
+   - W(x, y) is a normalization factor ensuring the weights sum to 1.

Intuitively, unlike linear filters (e.g., Gaussian blur) that average all nearby pixels, the bilateral filter selectively averages only those neighbors that are both **spatially close and similar in intensity**. As a result, pixels across edges (which have large intensity differences) receive negligible weights, preventing smoothing across boundaries. This allows the filter to **reduce noise in homogeneous regions while preserving sharp edges**, making it particularly effective for natural images where structure and detail must be retained.

Let us now create a noisy image from the following grayscale mountain image. The following code snippet demonstrates how to use the np.random_noise() function:

```
```python
im = color.rgb2gray(img_as_float(io.imread('images/mountain.png')))
sigma = 0.155
noisy = random_noise(im, var=sigma**2)
```
```

The next figure shows the noisy image, created by adding random noise to the original image.

![](bbox=[0.311, 0.544, 0.691, 0.752])

*Figure 7.26 – Noisy mountain image*

The following code snippet demonstrates how to use the bilateral filter to denoise the previous noisy image, with different values for the parameters, σ color and σ spatial. Note that sigma_color in scikit-image corresponds to σᵣ (intensity similarity), and sigma_spatial corresponds to σₛ (spatial proximity).