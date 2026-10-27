
The following figure shows the output of the code snippet:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_49_0.png)

Figure 1.9 – Astronaut and cameraman images from scikit-image data

## Reading and displaying multiple images at once

We can use the skimage.io module's imread_collection() function to load all images in a folder having a particular pattern in the filename and display them simultaneously with the imshow_collection() function. The following code block shows an example of reading and showing an image collection:

```
images = imread_collection('images/image_dir/*.jpg')
print(len(images)) # print no. of jpg images in the folder, there are 4
# 4
imshow_collection(images); plt.show()
```

The following figure shows the output of the preceding code block:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_49_1.png)

Figure 1.10 – Reading and displaying a collection of images with scikit-image

## Block views on images

Block views using skimage.util.view_as_blocks() split an image into non-overlapping patches to enable local operations such as pooling. Given a grayscale image partitioned into m × n blocks (after optional padding to ensure divisibility), each block is reduced to a single value to form a downsampled image: mean pooling computes the average intensity, max pooling selects the maximum intensity, and median pooling selects the median intensity within each block.

The next code block defines a couple of helper functions for visualization: plot_image() displays a single image with a title and axis turned off, while plot_images() arranges multiple images in a 1×n grid using matplotlib, applies grayscale colormap, sets individual subplot titles, and adds a global suptitle with adjustable figure and font sizes.

```
def plot_image(im, title='', fontsize=20):
    plt.imshow(im), plt.axis('off'),
    plt.title(title, size=fontsize)

def plot_images(images, titles, suptitle='', fontsize=20, \
                supfontsize=22, figsize=(15,7)):
    n = len(images)
    plt.figure(figsize=figsize)
    plt.gray()
    for i in range(n):
        plt.subplot(1,n,i+1), plot_image(images[i], titles[i], fontsize)
    plt.suptitle(suptitle, size=supfontsize)
    plt.tight_layout()
    plt.show()
```

The next code snippet uses different pooling methods for implementing a block view for an input image:

```
from skimage.util import view_as_blocks
im = color.rgb2gray(imread('images/sculpture.png')[..., :3])
block_shape = (8, 8) # Ensure image dimensions are divisible by block size
pad_height = (block_shape[0] - im.shape[0] % block_shape[0]) % block_shape[0]
pad_width = (block_shape[1] - im.shape[1] % block_shape[1]) % block_shape[1]
if pad_height != 0 or pad_width != 0:
    # optional: pad the image to make it divisible by block size
    im = np.pad(im, ((0, pad_height), (0, pad_width)), mode='reflect')
view = view_as_blocks(im, block_shape)
flatten_view = view.reshape(view.shape[0], view.shape[1], -1)
mean_view = np.mean(flatten_view, axis=2)
max_view = np.max(flatten_view, axis=2)
median_view = np.median(flatten_view, axis=2)
plot_images([im, mean_view, max_view, median_view], ['original', \
    'local mean pooling', 'local max pooling', 'local median pooling'])
```

If you run the preceding code, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_51_0.png)

Figure 1.11 – Block views on an image using different local pooling methods

The above code first loads an RGB image, discards any alpha channel, and converts it to grayscale for single-channel processing. It then divides the image into non-overlapping 8 × 8 blocks using view_as_blocks(), padding the image if required so that its dimensions are divisible by the block size.

The block representation reshapes the image from (H, W) into (H/8, W/8, 64), where each block is flattened into a 1D array of 64 pixels. Local pooling operations are then applied on each block: np.mean() computes average intensity (mean pooling), np.max() preserves the strongest responses (max pooling), and np.median() reduces noise while preserving structure (median pooling).

Finally, the plot_image() and plot_images() helper functions visualize single or multiple images with titles using Matplotlib, while plt.tight_layout() arranges subplots neatly.

## Reading, saving and displaying images with OpenCV-Python

With OpenCV-Python, reading both grayscale and color images is straightforward. The cv2.imread() function is used for reading images, and you can specify the color mode using optional flags. Here’s how to read a color image and display it inline inside the notebook. You need to provide the image file path to the cv2.imread() function, as shown in the next code snippet. Check that the image is successfully loaded (image is not None):

```
import cv2
image = cv2.imread('images/model.png')
cv2.imshow('Model', image)
cv2.waitKey(0)
cv2.destroyAllWindows()
print(image.dtype)
# uint8
```

> **Note**
> cv2.imshow() opens a GUI window and does not work reliably inside Jupyter Notebook—it should be run as a standalone Python script (e.g., via Terminal or a .py file).

Here’s how to read a grayscale image using the flag cv2.IMREAD_GRAYSCALE (which equals 0):

```
image_gray = cv2.imread('images/model.png', cv2.IMREAD_GRAYSCALE)
cv2.imshow('Model Gray', image_gray)
cv2.waitKey(0)
```

OpenCV reads color images in the blue, green, and red (BGR) color format by default. If you want to display the image using Matplotlib, which expects images in the RGB format, you should convert the color channels, using the cv2.cvtColor() function with the input flag as cv2.BGR2RGB to display colors properly, as demonstrated in the next code snippet:

```
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
plt.imshow(np.hstack(([image, image_rgb,
    np.repeat(image_gray[..., np.newaxis], 3, axis=-1)]))
```

If you run the preceding code snippet, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_52_0.png)

Figure 1.12 – BGR to RGB conversion for images loaded with OpenCV-Python

To write an image with OpenCV-Python, you can use the cv2.imwrite() function. The first argument of the function is the path to store, whereas the second argument is the image as a NumPy array. Here’s an example that converts the input image from RGB to the YUV color space and saves the image after color space conversion:

```
cv2.imwrite('images/model_yuv.png', cv2.cvtColor(image_rgb,
    cv2.COLOR_RGB2YUV))
# True # => image is successfully written (saved) to disk without error
```

As demonstrated in the preceding code snippet, the cv2.imwrite() function takes the file path and the image data as input and saves the image to the specified location. Ensure that the path includes the correct file extension (e.g., .jpg or .png) based on the desired image format. The format is determined by the file extension. If the file path doesn’t exist, OpenCV will attempt to create it. Remember that OpenCV supports various image formats, such as JPEG, PNG, BMP, and more. The format is determined based on the file extension provided in the file path.

## Reading and saving images with ImageIO

ImageIO is a simple Python library for image I/O that provides functions like `imageio.imread()` and `imageio.imwrite()` for reading and saving images in multiple formats, including support for animated GIFs by reading frames and generating GIFs from image sequences. An animated GIF consists of multiple frames played sequentially in a loop to create motion, typically uses a limited color palette (≤ 256 colors), supports transparency, and relies on lossless compression, making it widely compatible across platforms while maintaining compact file sizes.

### Extracting frames from animated GIFs

Frames from an animated GIF can be extracted in ImageIO using `imageio.get_reader()`, which creates a reader object that allows sequential iteration over the GIF’s frames, each represented as a NumPy array; these frames can then be processed or selectively sampled (e.g., randomly choosing a subset of frames using `np.random.rand()`), enabling inspection or manipulation of individual animation components.

```python
import imageio
gif_path = 'images/leaves.gif'
np.random.seed(3)
images = []
with imageio.get_reader(gif_path) as reader:
    for i, frame in enumerate(reader):
        if len(images) < 5:
            if np.random.rand() > 0.75:
                images.append(frame)
                print(f"Frame {i + 1} shape: {frame.shape}")
plt.imshow(np.hstack(images)), plt.axis('off')
plt.title('frames selected from the animated GIF', size=12); plt.show()
# Frame 5 shape: (512, 512, 4)
# Frame 6 shape: (512, 512, 4)
# Frame 26 shape: (512, 512, 4)
# Frame 30 shape: (512, 512, 4)
# Frame 31 shape: (512, 512, 4)
```

Here is the output you will obtain upon running the preceding code snippet:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_53_0.png)

*Figure 1.13 – Five frames selected from the animated GIF*
