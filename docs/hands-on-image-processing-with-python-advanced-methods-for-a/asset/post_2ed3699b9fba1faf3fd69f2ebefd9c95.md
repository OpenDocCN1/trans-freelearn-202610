
Let’s now plot the original image, the one obtained after applying `SimilarityTransform`, and the one obtained after applying `AffineTransform` with the inverse transformation matrix. As can be seen from the next figure, the original image is reconstructed except for the cropped regions (outside boundary), as expected:

```
```python
plot_images([im, im_tformed, im_recon_affine],\
            ['original', 'transformed with SimilarityTransform',\
             'reconstructed with AffineTransform'],\
            'Applying geometric transformations to an image',\
            fontsize=15, figsize=(15,5))
```
```

If you run the preceding code snippet, you should get a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_98_0.png)

*Figure 2.7 – Reconstructing image with affine transform with scikit-image*

## Applying the swirl transform

A swirl transform is a non-linear image transformation that introduces a swirling or twisting effect to an image. This type of transformation is characterized by a rotation of pixels around a specified center, resulting in a swirl-like pattern. The amount of swirl depends on parameters such as the center of rotation and the strength of the twist. This non-linear transform is defined by the documentation as follows.

Consider the coordinate $(x, y)$ in the output image. The reverse mapping for the swirl transformation first computes, relative to a center $(x_0, y_0)$, its polar coordinates, $\rho, \theta$, and then transforms them according to the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_98_1.png)

*Figure 2.8 – The formula for the swirl transform*

Here, *strength* is a parameter indicating the amount of swirl, *radius* indicates the swirl extent in pixels, and *rotation* adds a rotation angle. The transformation of *radius* into *r* is to ensure that the transformation decays to ≈ 1/1000<sup>th</sup> within the specified radius. Here, we shall use the `swirl()` function from the `skimage.transform` module.

```
```python
from skimage.transform import swirl

im_swirled = swirl(im, rotation=0, strength=5, radius=300)

plot_images([im_trans, im_rot, im_rot_center, im_sim, im_swirled], 
    ['translation', 'rotation', 'rotation (centered)', 'similarity transform', 'swirl transform'],
    'geometric transformation with warp()', figsize=(15,5))
```
```

Running the preceding code snippet should produce the following figure as output:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_99_0.png)

*Figure 2.9 – Geometric transform with scikit-image warp()*

## Applying piecewise affine transformation

A piecewise affine transformation divides an image into smaller regions and applies separate affine transformations to each region, enabling flexible localized warping. Using `PiecewiseAffineTransform` and `warp()` from `skimage.transform`, the transformation is estimated from corresponding source and destination control points generated on a regular grid using `np.linspace()`, which creates evenly spaced coordinate values across the image rows and columns. In the example, sinusoidal distortion is introduced by modifying the row coordinates with a cosine function, producing a wave-like deformation while preserving the column coordinates. The transformation is estimated with `tform.estimate(src, dst)` and applied using `warp(im, tform, output_shape=...)` to generate the warped image.

```
```python
rows, cols = im.shape[0], im.shape[1]

src_cols = np.linspace(0, cols, 20)
src_rows = np.linspace(0, rows, 10)
src_rows, src_cols = np.meshgrid(src_rows, src_cols)
src = np.dstack([src_cols.flat, src_rows.flat])[0]
```
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

Running the preceding code snippet should produce the following figure as output:

warped

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_100_0.png)

Figure 2.10 – Applying piecewise affine transformation with scikit-image

## Adding random Gaussian noise to images

Random Gaussian noise is a type of statistical noise that follows a normal (Gaussian) distribution and is often added to images to simulate real-world noise.

It is characterized by the **probability density function (PDF)**, *f*, as follows:

$$f(x)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

Here, we have the following:

- $x$ is the noise value
- $\mu$ (mean) controls the average brightness shift
- $\sigma$ (standard deviation) controls the spread of the noise; $\sigma^2$ is the variance

For an image with pixel values $I(x,y)$ at position $(x,y)$, Gaussian noise $N(x,y)\in\mathbb{R}$ is added as:

$$I_{noisy}(x,y)=I(x,y)+N(x,y),$$

where $I_{noisy}(x,y)$ is the noisy pixel intensity, and $N(x,y)\sim\mathcal{N}(\mu,\sigma^2)$ is the Gaussian noise sampled independently for each pixel.

These are the effects of Gaussian noise:

- Low $\sigma$: Subtle noise, barely visible
- High $\sigma$: Strong noise, degrading image quality
- Mean $\mu\ne 0$: Introduces a brightness shift in the image

We can use the `random_noise()` function from the `skimage.util` module to add different types (to be specified by the `mode` input parameter) of noise to an image. For `mode='gaussian'` (the default mode), the mean parameter specifies the mean ($\mu$, default 0), and `var` specifies the variance ($\sigma^2$) of the Gaussian distribution used to generate the noise. The noise is drawn from $\mathcal{N}(0,\sigma^2)$, as follows:

- `var=0.01` $\Rightarrow$ $\sigma=0.1$

> **Note to the Reader**
> 
> When using transformation functions like `estimate()` and `PiecewiseAffineTransform` in image warping tasks, there are important caveats to be aware of:
> 
> - The `estimate()` function does not raise an error for invalid inputs—such as mismatched point arrays or fewer than three control points. Instead, it silently fails and returns `False`, which can lead to `warp()` producing an unexpected or unchanged output. Always check the return value of `estimate()` and ensure you provide at least three non-collinear point pairs, as required for a valid local affine transformation.
> - While sinusoidal distortion highlights the flexibility of `PiecewiseAffineTransform`, it can introduce visible artifacts at triangle boundaries. These occur due to discontinuities between locally estimated affine transformations in adjacent triangles, and are a known limitation of this piecewise approximation method.

- var=0.25 ⇒ σ = 0.5
- Default: var=0.01 (i.e., σ = 0.1)

The next code snippet shows how (default) Gaussian noise with different variances can be added to an image:

```
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

Running the preceding code snippet should produce the following figure as output:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_102_0.png)

Figure 2.11 – Adding different levels of random normal noise to an image

## Note

The random_noise() function expects either a 2D grayscale image (H×W) or a 3D color image (H×W×3). If a grayscale image is mistakenly represented as (H, W, 3), noise will be applied independently to each channel, which may not match the intended behavior.

The mode parameter in random_noise() specifies the type of noise to add to the input image. It determines the noise model used. Here's a concise overview of a few available modes (gaussian is the default mode):

| Mode | Description |
|------|-------------|
| 'gaussian' | Adds Gaussian noise with zero mean and specified variance (var) |
| 'poisson' | Adds Poisson noise based on image values, useful for count-based data |
