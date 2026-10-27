
In machine learning pipelines, especially image classification or contrastive learning, such transformations can act as data augmentation techniques. Vignetting may introduce occlusion-like variance, improving model robustness to central-versus-peripheral feature importance. However, aggressive application may hinder models if it obscures critical information. Use this effect with care depending on the task.

> **Note:**
>
> Converting between PIL and NumPy repeatedly can introduce memory and processing overhead, especially when working with large images. For better performance, consider batch processing or using PIL-native operations in a chained manner to avoid unnecessary conversions.

## Generating images with PIL

PIL can be used to generate images of the following types:

- **Mandelbrot set:** The `effect_mandelbrot()` function generates an image of the Mandelbrot fractal by mapping the specified pixel grid to a region of the complex plane. This region is defined by the extent parameter, which determines the area of the complex plane to visualize. The parameters the function accepts are the following:
  - `size`: The requested size in pixels, as a 2-tuple: (width, height)
  - `extent`: The extent to cover, as a 4-tuple: (x0, y0, x1, y1)
  - `quality`: The quality of the image
- **Gaussian noise of size (256 x 256) centered around (128 x 128):** The `effect_noise()` function is used. The parameters it accepts are the following:
  - `size`: The requested size in pixels, as a 2-tuple: (width, height)
  - `sigma`: The standard deviation of noise
- **256x256 linear gradient from black to white, top to bottom:** The `linear_gradient()` function is used, accepting the parameter mode (input mode).
- **256x256 radial gradient from black to white, center to edge:** The `radial_gradient()` function is used, again accepting the mode parameter (input mode).

> **Tip:**
>
> Use `linear_gradient` for effects such as fades, shadows, and UI transitions, while `radial_gradient` is well suited for spotlight effects, vignettes, or circular masks in image compositing and lighting simulations.

The following code snippet demonstrates the generation of the images:

```python
from PIL.Image import effect_mandelbrot, effect_noise, \
                  linear_gradient, radial_gradient

sz = (256, 256)
plt.figure(figsize=(20,8))
plt.imshow(np.hstack((
    effect_mandelbrot(size=sz, \
                      extent=(-2, 1.5, 1, 1.5), quality=100),
    effect_noise(size=sz, sigma=5),
    linear_gradient(mode='L'),
    radial_gradient(mode='L'))))
```

If you run the preceding code snippet, you should obtain something similar to the following figure.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_155_0.png)

*Figure 3.14 – Generating images with PIL*

The preceding effects not only are useful for visual experimentation but also find practical applications in procedural texture generation (e.g., in game development, computer graphics, and simulations), synthetic datasets (e.g., in machine learning or computer vision), and testing image-processing pipelines (e.g., to benchmark algorithms due to known mathematical properties) where controlled patterns are beneficial.

> **Insight:**
>
> The **Mandelbrot** set is defined as the set of complex numbers, for which the iterative *sequence*, with *c*, does *not* diverge to infinity (i.e., remains bounded even when *n*).
>
> This Stackoverflow question https://stackoverflow.com/questions/78030547/how-to-make-faster-mandelbrot-set-plot answers how you can generate the set from scratch with Python. It is not only visually captivating due to its infinite complexity and self-similarity, but it’s also a fundamental construct in fractal geometry, often used to illustrate chaos theory and complex number dynamics.

Having explored the diverse capabilities of image manipulation with Python’s PIL library—from transforming image quality to applying artistic effects—we’ve unlocked a wide array of creative possibilities. But the journey doesn’t end here! In the next section, we’ll dive into the world of image manipulation with opencv-python, where a new set of tools and techniques awaits to push our imaginations even further.

## Image manipulation with opencv-python

As discussed earlier, opencv-python is a powerful and widely used library for image manipulation and computer vision tasks in the Python programming language. Renowned for its simplicity and efficiency, opencv-python empowers developers to perform a diverse range of image-processing operations, from basic transformations to advanced computer vision applications. In this section, you will learn how to implement a few image manipulation tasks with the library functions.

## Rotating

OpenCV provides efficient functions for performing image rotation through affine transformations. In the following Python demonstration, an input image is rotated both counterclockwise and clockwise by 45° about its center. The image is first loaded with cv2.imread() and converted from OpenCV’s default BGR color format to RGB using cv2.cvtColor() for correct visualization.

The rotation matrix is computed with cv2.getRotationMatrix2D(center, angle, scale), where center specifies the pivot point x,y, angle is the rotation angle in degrees (positive for counterclockwise rotation and negative for clockwise rotation), and scale controls image scaling (set to 1 for no scaling). Internally, the function returns the 2 × 3 affine transformation matrix:

$$M = \begin{bmatrix} \alpha & \beta & (1-\alpha)c_x - \beta c_y \\ -\beta & \alpha & \beta c_x + (1-\alpha)c_y \end{bmatrix}$$

Where $c_x, c_y$ are the coordinates of the rotation center, $\alpha = \cos\theta$, $\beta = \sin\theta$, $s$ is the scale factor, and $\theta$ is the rotation angle.

The transformation is then applied using cv2.warpAffine(src, M, dsize), where src is the input image, M is the 2 × 3 affine transformation matrix, and dsize=(width, height) specifies the output image dimensions. Additional optional parameters include flags for interpolation (e.g., INTER_NEAREST, INTER_LINEAR, INTER_CUBIC), borderMode for handling pixels outside the image boundary (e.g., BORDER_CONSTANT, BORDER_REPLICATE, BORDER_REFLECT), and borderValue for specifying the fill color when constant borders are used. The resulting images demonstrate clockwise and counter-clockwise rotations about the image center using affine warping.

```python
image = cv2.cvtColor(cv2.imread('images/fruits.png'), cv2.COLOR_BGR2RGB)
rotation_angle = 45
rows, cols = image.shape[:2]
rotation_matrix = cv2.getRotationMatrix2D((cols / 2, rows / 2), \
    rotation_angle, 1)
rotated_image_anticlockwise = cv2.warpAffine(image, \
    rotation_matrix, (cols, rows))
```

```python
rotation_matrix = cv2.getRotationMatrix2D((cols / 2, rows / 2), \
    -rotation_angle, 1)
rotated_image_clockwise = cv2.warpAffine(image, rotation_matrix, \
    (cols, rows))
plot_images([image, rotated_image_anticlockwise, \
            rotated_image_clockwise], \
    ['original', 'rotated counterclockwise', 'rotated clockwise'])
```

If you run the preceding code snippet, you should obtain something similar to the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_157_0.png)

Figure 3.15 – Rotating an image with opencv-python

## Avoiding cropping when rotating images

When rotating images using cv2.getRotationMatrix2D() and cv2.warpAffine(), the default output canvas size often remains the same as the original image. This can cause parts of the rotated image to be cropped, especially at larger rotation angles.

To prevent cropping, consider the following best practices:

- Calculate the new canvas size that fully contains the entire rotated image by accounting for rotation-induced width and height changes
- Adjust the rotation matrix's translation components to center the image within the enlarged canvas
- Use the updated rotation matrix and new canvas size when calling cv2.warpAffine()

This approach preserves all image content post-rotation, avoiding undesired clipping. If you visualize the rotated images with and without crop side by side along with the original image, you should obtain something like the following.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_157_1.png)

Figure 3.16 – Rotating an image with or without crop using opencv-python
