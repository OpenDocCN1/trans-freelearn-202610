struction and geometric transformations. In 1D, cubic spline interpolation fits a cubic polynomial between consecutive data points:

$S_i(x) = a_i + b_i(x - x_i) + c_i(x - x_i)^2 + d_i(x - x_i)^3$

where $a_i$, $b_i$, $c_i$ and $d_i$ are coefficients for the interval $[x_i, x_{i+1}]$. These coefficients are determined by enforcing continuity constraints such that the spline passes through all data points and its first and second derivatives remain continuous at the interval boundaries, ensuring smooth changes in intensity and curvature. Boundary conditions, such as natural splines ($S''(x) = 0$) at the endpoints, further constrain the solution and stabilize the interpolation near image boundaries. In 2D image interpolation, spline functions are extended along both the $x$- and $y$-directions to generate a smooth interpolated surface over the image grid.

Different interpolation methods involve trade-offs between computational complexity, smoothness, and reconstruction quality. **Nearest-neighbor interpolation** is computationally fastest and simplest to implement, but it often produces blocky and jagged artifacts, making it suitable for real-time rendering, segmentation masks, and discrete labels. **Bilinear interpolation** provides smoother intensity transitions with moderate computational cost, although noticeable blurring may occur during high upscaling. **Bicubic interpolation** generates smoother and more natural-looking images and is widely used in high-quality image resizing and photo editing, but it is computationally slower and may introduce ringing artifacts near sharp edges. **Spline interpolation** produces highly smooth reconstructions with continuous derivatives, making it particularly suitable for medical imaging, scientific visualization, and geometric transformations, although at significantly higher computational complexity. Now, let's demonstrate the impact of interpolation methods used to increase the size of an image with upsampling.

# Upsampling and interpolation with Scipy

The scipy.interpolate module can reconstruct an approximate image from sparsely sampled pixels using interpolation. In the following implementation, the function create_interpolated_image() randomly selects nsamples pixel locations from image, extracts their intensity values, and uses scipy.interpolate.griddata() to estimate the remaining pixel values over the complete image grid (Y, X) using linear interpolation:

```
def create_interpolated_image(nsamples):
    ix = np.random.randint(im.shape[1], size=nsamples)
    iy = np.random.randint(im.shape[0], size=nsamples)
    samples = im[iy,ix]
    int_im = interpolate.griddata((iy, ix), samples, (Y, X))
    return int_im
```

> **Important**
> 
> scipy.interpolate.griddata() returns NaN for grid points outside the convex hull of the sampled points, which may produce blank regions when sample density is low.