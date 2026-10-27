Here, we have the following:

+   - $r(x, y) = I_{input}(x, y)$ is the intensity of the pixel at location $(x, y)$ in the input image
+   - $T(.)$ is the transformation (transfer) function applied to the pixel
+   - $s(x, y) = I_{output}(x, y)$ is the resulting intensity of the pixel at location $(x, y)$ in the output image

Here, we explore a couple of transformations that apply a transfer function to each single pixel value from the input image to obtain the corresponding pixel value for the output image, using the Image.point() method. Each pixel has a value between 0 to 255, inclusive.

# Log transformation

Logarithmic transformation is a nonlinear point operation used to compress the dynamic range of an image while enhancing details in darker regions. It is defined as follows:

$I_{output}(x, y) = c \cdot \log\left(1 + I_{input}(x, y)\right)$

Here, c is a scaling constant.

The next line of code uses the point() mtehod to apply a lambda transformation independently to each pixel intensity x. The pixel values are first normalized to the range [0,1] using x/255 transformed with log(1 + x/255) to avoid the undefined log(0), and finally rescaled back to the 0-255 range. This operation brightens dark pixels, compresses high intensities, and reduces the overall intensity range of the image.

```
im_g.point(lambda x: 255*np.log(1+x/255)).show()
```

# Power-law transformation

The power-law transformation for an image can be described mathematically as follows:

$s = c \cdot r^{\gamma}$

Here, we have the following:

+   - $r$ is the input pixel intensity (normalized between 0 and 1)
+   - $s$ is the output pixel intensity after the transformation
+   - $c$ is a constant that typically normalizes the result, often set to 1 (for simplicity) or 255 if the image is in 8-bit depth

Let’s look at the explanation:

+   - $\gamma$ is the gamma value, which controls the degree of transformation
+   - If $\gamma < 1$, the transformation enhances the contrast in the darker regions of the image (brightens the image)
+   - If $\gamma > 1$, the transformation reduces the contrast in the darker regions (darkens the image)
+   - If $\gamma = 1$, the image remains unchanged