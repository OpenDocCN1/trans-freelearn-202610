
The demonstrated effects include `blue_shift()` (moonlight-like blue tint), `charcoal()` (sketch effect), `colorize()` and `tint()` (color blending), `vignette()` (darkened borders), `sepia_tone()` (vintage appearance), `implode()` (pulls pixels toward the center), `polaroid()` (adds a Polaroid-style frame and shadow), `swirl()`, `wave()`, `shade()` (3D shading effect), `white_balance()`, `cycle_color_map()`, and `spread()`. The code loads an image with `wand.image.Image`, applies each effect with its corresponding parameters, and displays the original and transformed results in a Matplotlib grid, illustrating the variety of visual effects available through Wand and ImageMagick.

Now, refer to the following code snippet:

```python
from wand.image import Image as ImageW
from wand.color import Color


def call_func(img, func_name):
    img = img.clone()
    func = getattr(img, func_name)
    arg_dict = functions[func_name]
    func(**arg_dict)
    return img

functions = {
    'spread': {'radius':16.0},
    'blue_shift': {'factor':2},
    'charcoal': {'radius':2, 'sigma':1},
    'colorize': {'color': 'magenta', 'alpha':"rgb(10%, 50%, 20%)"},
    'tint': {'color': 'magenta', 'alpha': 'rgb(50%, 70%, 80%)'},
    'vignette': {'sigma':3, 'x':15, 'y':15},
    'sepia_tone': {'threshold': 0.9}, 'implode': {'amount':0.5},
    'polaroid': {}, 'swirl': {'degree': -120},
                 'wave': {'amplitude':10, 'wave_length':50},
    'shade': {'gray':True, 'azimuth':100.0, 'elevation':45.0},
    'white_balance': {},
    'cycle_color_map': {'offset': 50}
}

with ImageW(filename="images/parrot.png") as img:
    plt.subplot(3,5,1), plt.imshow(np.array(img))
    plt.axis('off'), plt.title('original', size=20)
    i = 2
    for func_name in functions:
        out_img = call_func(img, func_name)
        plt.subplot(3,5,i), plt.imshow(np.array(out_img))
```

```python
with ImageW(filename="images/parrot.png") as img:
    filtered_img = img.fx(fx_filter)
```

If you run the preceding code snippet, you should obtain something similar to the following.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_175_0.png)

*Figure 3.28 – Applying sketch, solarize, and fx_filter with Wand*

## Distorting and undistorting images

Image distortion commonly arises from lens effects, with radial distortion causing straight lines to appear curved. Distortion can be modeled mathematically (e.g., with the Brown–Conrady model) using radial and tangential distortion coefficients. Distortion is applied by mapping original pixel coordinates to distorted coordinates, while undistortion reverses this process using known distortion parameters, typically obtained through camera calibration (e.g., using checkerboard images). Libraries such as OpenCV and Wand provide tools for distortion correction.

The next code snippet uses Wand's `Image.distort()` method to simulate several geometric distortions:

- `distort('barrel', ...)`: Applies radial barrel distortion, causing straight lines to bulge outward.
- `distort('polar', ...)`: Transforms the image between Cartesian and polar coordinate systems, producing a radial warping effect.
- `distort('arc', (ArcAngle, RotateAngle))`: Bends the image along a curved arc.
- `distort('shepards', ...)`: Applies Shepard's (Inverse Distance Weighting) distortion, where pixel positions are warped using distance-based interpolation weights.

Additional settings such as `virtual_pixel='transparent'`, `artifacts['distort:viewport']`, and `artifacts['shepards:power']` control boundary handling and distortion behavior. The resulting images demonstrate various geometric warping effects, including bulging, radial, arched, and spatially interpolated deformations.

```python
with ImageW(filename='images/house.png') as im:
    w, h = im.size
    im.virtual_pixel = 'transparent'  # 'black'
    im_out_barrel = im.clone()
    im_out_barrel.distort('barrel', (0.2, 0.0, 0.0, 1.0))
    im_out_barrel.save(filename='images/house_distorted.png')
```

If you run the preceding code snippet, you should obtain something similar to the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_177_0.png)

*Figure 3.30 – Undistorting an image with Wand*

Now that you’ve explored how to craft compelling visual effects and perform advanced image transformations using Wand, it’s time to switch gears. In the next section, we’ll dive into image manipulation with Matplotlib, where data visualization meets creative image processing.

## Image manipulation with Matplotlib

### Adding annotations to an image

You can add annotations such as text and arrows (with the `text()` and `arrow()` functions from the `matplotlib.pylab` module) to enhance the visualization of an image, as shown in the following example:

- `plt.text()` places the text Packt at the specified coordinates (310,310) in red and with a font size of 40.
- `plt.arrow()` adds an arrow starting from the point (300, 225) with a specified length and angle. It is blue and has a width of 5, and the arrowhead has a width of 15.

Now, refer to the following code snippet:

```python
im = plt.imread('images/parrot.jpg')
plt.imshow(im)
plt.text(310, 310, 'Packt', color='red', fontsize=40, alpha=0.5)
plt.arrow(300, 225, 25, 25, color='blue', width=5, \
                              head_width=15)
```

If you run the preceding code snippet, you should obtain something similar to the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_178_0.png)

*Figure 3.31 – Annotating an image with text*

### Drawing contour lines for an image

A contour line for an image is a curve connecting all the pixels where they have the same particular (constant) value. If we represent the image as a function, f(x, y), where x and y are the spatial coordinates, a contour line corresponds to a set of points (x, y) such that f(x, y) = constant.

The following code snippet shows how to draw the contour lines for a gray-scale image of Einstein, using the `contour()` function from the `matplotlib.pylab` module.

The constant levels are specified using the `np.linspace()` function, which creates an array with 15 values, evenly spaced between 0 and 255 (inclusive), to be used as levels (constants) for the contour plot.

Contour lines can be styled and colored using various parameters, such as `colors`, `cmap` (colormap; here `plt.cm.hot` is used), and `linewidths`. You can fill the regions between contours using `plt.contourf()`.

You can add labels to the contours using the `plt.clabel()` function. This adds labels to the contours at specified locations (as can be seen from the following figures, the 18.2, 36.4, and 91.1 levels are added as labels. These values were generated previously using `np.linspace()`).

Now, refer to the following code snippet:

```python
im = imread("images/einstein.jpg")  # read image as
                                    # a numpy ndarray

plt.subplot(131), plt.imshow(im, cmap='gray', aspect='auto')
plt.axis('off'), plt.title('Original Image', size=20)
plt.subplot(132)
cs = plt.contour(np.flipud(im), levels=np.linspace(0,255,15), \
                 cmap=plt.cm.hot)
plt.clabel(cs, fontsize=15, inline=True)
```

```python
plt.gray()
plt.subplot(131), plt.imshow(im_ctr, aspect='auto')  # PIL
plt.subplot(132), plt.imshow(im_gray, aspect='auto')  # cv2
plt.subplot(133), plt.imshow(im, aspect='auto', alpha=0.25)
# skimage
for contour in contours:
    plt.plot(contour[:, 1], contour[:, 0], linewidth=2)
```

If you run the preceding code snippet, you should obtain something similar to the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_180_0.png)

*Figure 3.33 – Drawing contour lines on an image with PIL, scikit-image, and opencv-python*

Compare the contour plots obtained with three different libraries in the preceding figure.

## Image manipulations with Pilgram

Pilgram filters work by adjusting colors, contrast, brightness, and other properties. They work by manipulating pixel values using mathematical transformations, such as convolution, color mapping, and blending.

Mathematically, a filter can be expressed as a function, F, applied to an image matrix, I' = F(I), where we have the following:

- I is the original image represented as a matrix of pixel values
- F is the transformation function (filter effect)
- I' is the resulting filtered image
