To reduce aliasing, the image is resized separately using Lanczos resampling, using the next code snippet. Unlike nearest-neighbor interpolation, resample=Image.LANCZOS uses a truncated sinc-based Lanczos filter that combines neighboring pixels to preserve image detail while suppressing high-frequency components responsible for aliasing.

```
Factor, crop_coords = 6, (0, 200, 70, 270)
im_rsz = im.resize((int(im.width/factor), int(im.height/factor)),
    resample=Image.NEAREST, box=crop_coords)
im_rsz_antialiased = im.resize((int(im.width/factor),
    int(im.height/factor)), resample=Image.LANCZOS, box=crop_coords)
# plot im, im_rsz and im_rsz_antialiased here
```

If you run the preceding code snippet, you should obtain a figure like the following:

![](bbox=[0.14, 0.35, 0.82, 0.48])

Figure 4.10 – Downsampling with anti-aliasing using Lanczos with PIL

As can be seen from the preceding output, the Lanczos resampling method almost got rid of the artifact, whereas the nearest neighbor introduced Moire artifacts.

# Downsampling and anti-aliasing with scikit-image

Direct downsampling often produces aliasing artifacts because high-frequency image details exceed the Nyquist limit and become incorrectly represented at lower resolutions. For example, reducing both image dimensions by a factor of 6 causes one output pixel to represent 6 × 6 = 36 input pixels, making simple pixel sampling insufficient for accurate reconstruction.

Aliasing can be reduced either by increasing the sampling frequency or by suppressing high-frequency components before sampling using low-pass filtering. Anti-aliasing performs this smoothing operation, typically by convolving the image with a Gaussian low-pass filter before downsampling. The next code uses skimage.transform.rescale() function to demonstrate this effect: it rescales the input image UMBC with and without anti-aliasing, and compare the results to observe which one overcomes the aliasing problem.

```
im = imread('images/umbc.png')
scale, anti_aliasing = 0.3, False # True
im_down = rescale(im, scale = scale, channel_axis=-1, \
    anti_aliasing= anti_aliasing)
```