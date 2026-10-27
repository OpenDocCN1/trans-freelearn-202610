
An animated GIF can be created using ImageIO by first loading a sequence of images (e.g., PNG files from a folder via glob.glob() and imageio.imread()) into a list of frames, then initializing a GIF writer with imageio.get_writer() where the duration parameter controls per-frame display time, and finally iterating through the frame list to build the animation using writer.append_data(), producing an output file such as world.gif.

Now, refer to the following code snippet:

```
from glob import glob
# read frames for the animation
frames = [imageio.imread(f) for f in glob('images/world/*.png')]
# Save frames as an animated GIF
with imageio.get_writer('images/world.gif', duration=0.1) as writer:
    for frame in frames:
        writer.append_data(frame)
```

## Image file formats and conversion in Python

Images can be stored in multiple file formats, each with different compression strategies, color support, and use cases. Common formats include BMP (uncompressed raster, large size, supports 8/24/32-bit color depth with optional alpha channel), PNG (lossless compression, supports transparency, ideal for web graphics and sharp edges), JPEG (lossy compression, optimized for photographs with smooth gradients but no transparency), GIF (limited to 256 colors, supports lossless compression and animation), PPM/PNM (simple uncompressed formats with very large file sizes, rarely used), and TIFF (high-quality, flexible format supporting lossless compression, layers, and metadata, widely used in professional imaging).

The following table illustrates how bit depth affects pixel representation for a 2 x 2 image:

| Bit Depth | Pixel Values (Decimal/Binary) | Image |
|---|---|---|
| 8-bit | [[0, 85], [170, 255]]<br>Binary: [[00000000, 01010101], [10101010, 11111111]] | [2x2 grayscale image] |
| 16-bit (scaled to 8-bit for display) | [[0, 21845], [43690, 65535]]<br>Binary: [[0000000000000000, 0101010101010101], [1010101010101010, 1111111111111111]] | [2x2 grayscale image] |

Table 1.1: Bit depth and pixel representation

## Converting file formats

Despite format differences, Python image libraries abstract away metadata and allow direct access to image arrays and properties like size, mode, and data type. Format conversion can be performed using PIL by saving an image with a new extension (e.g., PNG to JPG), as demonstrated by the next code snippet:

```
im = Image.open("images/parrot.png")
print(im.mode)
# RGB
im.save("images/parrot.jpg")
```

PNG images in RGBA mode must first be converted to RGB before saving as JPEG, since JPEG does not support alpha channels, as shown in the next code snippet.

```
im = Image.open("images/hill_alpha.png")
print(im.mode)
# RGBA
im.convert('RGB').save("images/hill.jpg") # first convert to RGB mode
```

## Image types (modes)

Images can be categorized based on number of channels used to represent each pixel.

- Single-channel images: Each pixel is represented by a single value:
  - Binary (monochrome) images: Each pixel is stored using a single bit (0 or 1).
  - Grayscale images: Each pixel typically uses 8 bits with intensity values in the range [0,255].
- Multi-channel images: Each pixel is represented by a tuple of values, for example:
  - RGB / HSV images: Pixels are represented as (R, G, B) for red, green, and blue intensities, and as (H, S, V) for hue, saturation (colorfulness), and value (brightness), with HSV offering a more perceptually aligned color model.
  - RGBA images: Four-channel images represented as (r, g, b, α), where α denotes transparency.
  - Indexed color (palette) images: Pixels store indices into a color palette instead of direct RGB values. This reduces storage and is commonly used in GIF images.

## Converting image modes with PIL

PIL/Pillow supports multiple image modes, including:

- 1: 1-bit pixels, black and white, stored with one pixel per byte
- L: 8-bit pixels, black and white
- P: 8-bit pixels, mapped to any other mode using a palette
- RGB: 3 x 8-bit pixels, true color
- RGBA: 4 x 8-bit pixels, true color with transparency
- CMYK: 4 x 8-bit pixels, color separation
- F: 32-bit floating-point pixels

## From RGB to binary mode

We can use the convert() method from PIL.Image to convert an RGB image to a binary image (mode = '1') using the following line of code:

```
Image.open('images/parrot.png').convert('1', dither=Image.FLOYDSTEINBERG)
```

If you run the preceding line of code, you should obtain an image like the next one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_56_0.png)

Figure 1.14 – Binary image with Floyd-Steinberg dithering

## Note

The preceding binary image uses only two different pixel values (namely, black and white, and if converted to a NumPy ndarray, it produces an array of bool), but it still looks like the original image without much loss of information since it uses the dithering algorithm (FLOYD-STEINBERG, in this case), which we will discuss in detail later in Chapter 6 (the Image Enhancement chapter):

```
np.unique(np.array(Image.open('images/parrot.png').convert('1')))
# array([False, True], dtype=bool)
```

## From RGB to palette mode and back

In palette ("P") mode, pixels store indices that map to RGB colors in a lookup table. Each pixel's value is not a color directly but a reference (e.g., an integer index from 0–255) to a separate palette list, which contains the actual RGB values. This mapping helps compress the image size when the total number of colors is small, which is why formats such as GIF use this technique. The following table visually illustrates how palette mode works for a 2 x 2 (RGB) color image, and how each pixel in the image holds an index value that maps to an actual color in a palette:

## Note

The preceding code can be vectorized using NumPy and the for loop can be eliminated, which would offer better performance and more concise syntax:

```
palette_array = np.array(palette, dtype=np.uint8)
modified_palette = (palette_array + 19) % 256
image_palette.putpalette(modified_palette.tolist())
```

You can get the original and modified pixel data using the getdata() and putdata() methods, respectively; try them as an exercise.

## Adding an alpha channel to a PNG image (RGB to RGBA)

An alpha channel represents transparency. In the next code snippet, the putalpha() method applies a grayscale mask as transparency. The image parrot.png is loaded and converted to RGBA. The image Ishihara.png (often used to detect color blindness) is loaded and converted to grayscale (L mode), inverted using ImageOps.invert(), resized, and used as the alpha mask. Then putalpha() applies the transparency mask, the result is saved as parrot_alpha.png. The brighter mask regions become more opaque, while darker regions become more transparent.

```
from PIL import ImageOps
im = Image.open('images/parrot.png').convert('RGBA')
im_alpha = ImageOps.invert(Image.open("images/Ishihara.png").convert('L'))
im.putalpha(im_alpha.resize((im.width, im.height)))
im.save('images/parrot_alpha.png')
im # run in jupyter notebook
# RGB <ImagingCore object at 0x0000015375A334F0>
```

If you run the preceding code snippet, you should obtain a figure like the next one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_58_0.png)

Figure 1.16 – Adding Ishihara image intensity as alpha channel value to the parrot image with PIL

## Converting modes with scikit-image

Using `skimage.io.read(..., as_gray=True)` reads an image directly as 2D grayscale:

```python
im = imread("images/parrot.png", as_gray=True)
```

The `color.rgb2gray()` function can also convert RGB images to grayscale:

```python
im = imread("images/Ishihara.jpeg")
im_g = color.rgb2gray(im)
# Plot color and grayscale Ishihara image (im, im_g) here side-by-side
```

However, grayscale conversion may lose important color information. For example, in Ishihara color-blindness test plates, digits visible in the RGB image may become nearly invisible after grayscale conversion, as shown in the next figure.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_59_0.png)

*Figure 1.17 – Image for a test of color blindness, converted to grayscale with skimage.color*

## Image data types and conversion

In Python image processing, images are stored as arrays whose **data types** determine pixel value ranges, numerical precision, memory usage, and arithmetic behavior. The following table summarizes common image (NumPy) data types:

| Data Type | Description | Value Range | Usage Examples |
|---|---|---|---|
| `uint8` | 8-bit unsigned integer | 0 to 255 | Standard color images |
| `float32` | 32-bit float | 0.0 to 1.0 (normalized) | Normalized processing |
| `bool` | Boolean | True or False | Binary masks |

Table 1.3: Common image data types

## Inspecting the data type of an image

Here’s an example code:

```python
im = imread('images/parrot.png')  # automatically returns uint8
print("Image dtype:", im.dtype, "range", im.min(), "to", im.max())
# Image dtype: uint8, range: 0 to 255
```
