
Convert the RGB image to Lab, modify one of the channels, and convert it back to RGB:

```python
def add_channel_lab(im, i, x):
    im_lab = color.rgb2lab(im)
    im_lab[...,i] = x + im_lab[...,i]
    return color.lab2rgb(im_lab)

im = imread('images/scene.jpg')
plot_images([im, add_channel_lab(im, 0, 50), \
             add_channel_lab(im, 1, 10), \
             add_channel_lab(im, 2, -25)], \
            ['original', 'luminosity changed', 'red tinted', 'cooled'])
```

If you run the preceding code snippet, you should obtain a figure like the next one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_70_0.png)

*Figure 1.31 – Color shifting in the Lab color space*

## Skin color detection with the HSV and YCbCr color spaces

Skin color detection identifies pixels lying within predefined skin-tone ranges in different color spaces. The following implementation uses both HSV and YCbCr spaces to improve robustness. The input BGR image is converted using `cv2.cvtColor()`, and binary skin masks are generated with `cv2.inRange()` using predefined lower and upper thresholds:

$$HSV_{lower}, HSV_{upper},\quad YCbCr_{lower}, YCbCr_{upper}$$

Pixels within the specified ranges are marked as skin (1), while others are suppressed (0). Morphological opening (`cv2.morphologyEx()`) removes small noise regions, and `cv2.medianBlur()` smooths the final mask. The two masks are combined using `np.bitwise_and()` to obtain a more reliable global skin mask.

The function `detect_skin_color()` accepts a BGR image (`np.ndarray`) and returns the original RGB image along with HSV-based, YCbCr-based, and combined skin-detection outputs. Type hints improve readability and editor-assisted error checking, although they are optional in Python.

The final visualization displays the original image together with the detected skin regions from HSV, YCbCr, and the combined mask side-by-side using Matplotlib.

```python
# Constants for HSV and YCrCb skin detection thresholds
HSV_LOWER, HSV_UPPER = (0, 15, 0), (17, 170, 255)

YCrCb_LOWER, YCrCb_UPPER = (0, 135, 85), (255, 180, 135)

def detect_skin_color(img: np.ndarray) -> tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]:
    img_HSV = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
    HSV_mask = cv2.inRange(img_HSV, HSV_LOWER, HSV_UPPER)
    HSV_mask = cv2.morphologyEx(HSV_mask, cv2.MORPH_OPEN, np.ones((3,3), np.uint8))
    img_YCrCb = cv2.cvtColor(img, cv2.COLOR_BGR2YCrCb)
    YCrCb_mask = cv2.inRange(img_YCrCb, YCrCb_LOWER, YCrCb_UPPER)
    YCrCb_mask = cv2.morphologyEx(YCrCb_mask, cv2.MORPH_OPEN, np.ones((3,3), np.uint8))
    global_mask = np.bitwise_and(YCrCb_mask,HSV_mask)
    global_mask = cv2.medianBlur(global_mask,3)
    global_mask = cv2.morphologyEx(global_mask, cv2.MORPH_OPEN, np.ones((4,4), np.uint8))
    img_RGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    out_HSV = cv2.cvtColor(img & cv2.merge([HSV_mask]*3), cv2.COLOR_BGR2RGB)
    out_YCrCb = cv2.cvtColor(img & cv2.merge([YCrCb_mask]*3), cv2.COLOR_BGR2RGB)
    out_global = cv2.cvtColor(img & cv2.merge([global_mask]*3), cv2.COLOR_BGR2RGB)
    return img_RGB, out_HSV, out_YCrCb, out_global

img = cv2.imread("images/cycling.png")
img_RGB, out_HSV, out_YCrCb, out_global = detect_skin_color(img)
plt.imshow(np.hstack((img_RGB, out_HSV, out_YCrCb, out_global)))
```

If you run the preceding code snippet, you will get the following figure as output:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_71_0.png)

*Figure 1.32 – Skin color detection with different color spaces*

> **Note**
>
> The function explicitly uses **type hints** (e.g., `img: np.ndarray` and the `tuple[np.ndarray, ...]` return type). Type hints (introduced in Python 3.5) improve code readability, enable better error checking and autocompletion in modern editors such as VS Code or PyCharm, and make the function’s input-output expectations clearer, especially in collaborative or large-scale projects. However, as Python is inherently a dynamically typed language (i.e., types are resolved at runtime), type hints are optional and don’t impact the functionality of the code at runtime.

As we have already discussed, PIL uses an Image object to store an image, whereas scikit-image uses a NumPy ndarray data structure to store the image data. The next section describes how to convert between these two data structures.

## Converting the image data structures

The following code block shows how to convert from a PIL Image object to a NumPy ndarray (to be consumed by scikit-image):

```python
im = Image.open('images/flowers.jpg')  # read image into Image object (PIL)
im = np.array(im)  # create a numpy ndarray from the Image object
```

The following code block shows how to convert from a NumPy ndarray to a PIL Image object. When run, the code shows the same output as the preceding figure:

```python
im = imread('images/flowers.png')  # read image into numpy ndarray
im = Image.fromarray(im)  # create a PIL Image object from the numpy ndarray
#im.show() # display the image with PIL Image.show() method
```

Now that we have seen how to load an image into memory, let us proceed to a few basic image manipulation methods.

## Image array coordinates versus plotting coordinates

When working with images in Python, it’s important to understand that **array indexing** and **plotting coordinates** do not always align in how they represent spatial data. This difference can lead to confusion when visualizing or processing images, especially in tasks such as pixel manipulation, drawing, or overlaying graphics.

### Image array coordinates (NumPy-style)

In NumPy, an image is treated as a multi-dimensional array. This means the following:

- The first index refers to the row (Y axis), going from top to bottom; the second index refers to the column (X axis), going from left to right; the origin, `(0,0)`, is the top-left pixel of the image; Y (row) increases as you move downward; X (column) increases as you move right.

### Plotting coordinates (Matplotlib-style)

Matplotlib’s general plotting functions (e.g., `plt.plot()` and `plt.scatter()`) use standard Cartesian coordinates: the origin, (0, 0), is at the bottom left by default, X increases to the right, and Y increases upward. However, when displaying images with `plt.imshow()`, the image is still displayed top-down (origin at top left) unless overridden. The next table summarizes the differences in the coordinate systems:

| Feature | NumPy Array | Matplotlib Plot |
|---|---|---|
| Indexing format | (row, col) = (y, x) | (x, y) |
| Origin (default) | Top left | Bottom left |
| Y axis direction | Top to bottom | Bottom to top (unless origin='upper') |
| Use | Processing and filtering | Display and overlays |

Table 1.4: Coordinate system for numpy 2D array vs. matplotlib plot

## Basic image manipulation

Different Python libraries can be used for basic image manipulation. Almost all the libraries store an image in a NumPy ndarray (2D array for grayscale, 3D array for RGB image, for example). As explained earlier, the following image shows the positive x and y directions (origin being the top-left corner of the image 2D array) for the colored Mona Lisa image:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_73_0.png)

*Figure 1.33 – Image versus NumPy 2D array coordinates*

### Common image manipulation operations

Digital image manipulation involves modifying images to enhance appearance, change composition, or create visual effects. Common operations include cropping, resizing, rotating, color adjustment, sharpening, blurring, warping, and applying filter effects such as sepia or grayscale. More advanced manipulations include adding text, removing objects, cloning image regions, and merging multiple images into composite scenes. Many of these techniques will be implemented throughout this book using different Python image-processing libraries.

### Image manipulation with NumPy

NumPy is a powerful library for numerical and array operations in Python, and it can be used for image manipulation due to its array-handling capabilities. Here, in the following subsections, are some basic image manipulation tasks using NumPy.

#### Cropping with array slicing

NumPy array slicing refers to the process of extracting a portion of an array or creating a view of the original array with a subset of its elements. The next code snippet shows how slicing and masking with a NumPy array can be used to first crop the face region from the input Mona Lisa image and then hide a portion of the image by applying a circular mask:

```python
mona = plt.imread("images/monalisa.jpg") # read from disk as numpy ndarray
lx, ly = mona.shape[:2]
cropped_mona = mona[120:210, 160:220, :]  # crop a region
X, Y = np.ogrid[0:lx, 0:ly]
mask = (X - lx / 2) ** 2 + (Y - ly / 2) ** 2 > lx * ly / 4
masked_mona = mona.copy()
masked_mona[np.where(mask)] = 0 # masks
# plot mona, cropped_mona and masked_mona side-by-side here
```

If you run the preceding code snippet, you should get a figure like the following one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_74_0.png)

*Figure 1.34 – Cropping the Mona Lisa image with NumPy array slicing*

#### Adjusting brightness and contrast in a color image

Contrast and brightness adjustments are essential operations that help to enhance images or correct poor exposure. Contrast adjustment amplifies the differences between light and dark regions, while brightness adjustment alters the overall luminance of the image. Brightness and contrast adjustments are commonly performed in the Lab color space, where luminance is separated from color information. An RGB image is converted to Lab using `rgb2lab()`, modified through the luminance channel (L), and converted back to RGB using `lab2rgb()`.
