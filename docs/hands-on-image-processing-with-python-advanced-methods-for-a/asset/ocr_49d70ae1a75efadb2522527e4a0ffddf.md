Running the preceding code snippet should produce the following figure as output:

![](bbox=[0.307, 0.103, 0.787, 0.355])

*Figure 2.13 – Computing the CDF of the RGB color channels of the parrot image*

# Resizing/Rescaling an image

Image resizing and rescaling in `skimage.transform` are performed using the `resize()` and `rescale()` functions, which use interpolation (bicubic by default) to estimate pixel values in the transformed image. `rescale()` changes image size using scaling factors, while `resize()` directly specifies the output dimensions. During downsampling, enabling `anti_aliasing=True` applies Gaussian smoothing to reduce aliasing artifacts such as jagged edges and moiré patterns caused by insufficient sampling of high-frequency details. The `downscale_local_mean()` function performs downsampling through local averaging over non-overlapping blocks, producing smoother reduced-resolution images with minimized aliasing effects, as demonstrated by the next code snippet.

```
```python
from skimage.transform import rescale, resize, downscale_local_mean

image = color.rgb2gray(imread('images/model.png')[...,:3])
image = image / image.max()

image_rescaled = rescale(image, 0.25, anti_aliasing=True)
image_resized = resize(image, \
                       (image.shape[0] // 4, image.shape[1] // 4), \
                       anti_aliasing=True)
# assumption: image dimensions are divisible by blocksize (4,3) for
# downscale_local_mean() without padding
image_downscaled = downscale_local_mean(image, (4, 3))

print(image.shape, image_rescaled.shape, image_resized.shape, \
```
```