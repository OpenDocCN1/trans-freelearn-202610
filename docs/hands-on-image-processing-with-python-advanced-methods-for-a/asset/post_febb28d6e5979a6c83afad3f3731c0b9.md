
### The original and the deformed image

If you run the preceding code snippet, you should obtain a figure like the following one. It creates an organic-looking distortion, making the image look warped or undulating.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_143_0.png)

Figure 3.3 – The original and the deformed image

## Colorizing a grayscale image

The next code snippet colorizes a grayscale image by mapping its pixel intensities to a specified color gradient. The `ImageOps.colorize()` method maps black pixels to red (255,0,0) and white pixels to yellow (255,255,127), creating a smooth transition for midtones. This effectively simulates false-color mapping, enhancing grayscale images for better visual interpretation:

```python
im = Image.open('images/sculpture.png').convert('L')
im_colorized = ImageOps.colorize(im, black=(255,0,0), \
    white=(255,255,127))
plt.imshow(np.hstack((color.gray2rgb(np.array(im)), \
    np.array(im_colorized))))
plt.axis('off')
plt.title('The original and the colorized image', size=20)
```

If you run the preceding code snippet, you should obtain a figure like the following one.

### The original and the colorized image

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_143_1.png)

Figure 3.4 – The original and the colorized image

**Note:** `ImageOps.colorize()` expects a grayscale image with pixel values covering the full [0, 255] range. If the input image is normalized or has low contrast, the resulting colorization may appear flat or lack vibrancy. To achieve visually rich colorization, consider applying contrast stretching or rescaling the intensity values before using `ImageOps.colorize()`.

## Changing image colors from LUT files

A LUT is a predefined mapping that transforms input colors to new output colors, used for color grading and image enhancement. The next code snippet applies color grading to an image using a LUT file to change its colors. The .cube file contains predefined color transformations (RGB-to-RGB mappings) that map input colors to output colors (used in color grading). The original image is loaded and the LUT filter is applied to the image, changing its colors based on the predefined mapping.

This technique is widely used in photo editing and filmmaking to apply specific color tones, such as cinematic looks or mood enhancements:

```python
lut = load_cube_file("images/Bourbon 64.cube")  # NightFromDay.CUBE
im = Image.open("images/sea_beach.jpeg")
im_in = np.array(im)
im_out = np.array(im.filter(lut))
plt.imshow(np.hstack((im_in[:, :im_in.shape[1]//2], \
    im_out[:, :im_out.shape[1]//2])))
plt.axis('off')
```

If you run the preceding code snippet, you should obtain a figure like the next one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_144_0.png)

Figure 3.5 – Changing image color with LUT files

## Applying gamma (γ) correction with LUT

Gamma correction is a nonlinear intensity transformation used to adjust image brightness, particularly for enhancing dark images. A Lookup Table (LUT) provides an efficient implementation by precomputing the mapping between input and output pixel values, avoiding per-pixel computations.

### With PIL

With PIL, `ImageFilter.Color3DLUT.generate()` creates a 3D color LUT (for example, 11 × 11 × 11) using a callback function that applies the gamma transformation independently to the RGB channels. The generated LUT is then applied to the image using `Image.filter()`, enabling fast color correction without explicit pixel iteration:

```python
γ = 0.4
img = Image.open('images/roma.jpg')
LUT = ImageFilter.Color3DLUT.generate((11,11,11), \
    lambda r, g, \
    b: tuple(min(max(c ** γ, 0), 1) for c in (r, g, b))
)
img_out = img.filter(LUT)
plt.imshow(np.hstack((np.array(img), np.array(img_out))))
plt.axis('off')
plt.title('original and γ-corrected image', size=20)
```

If you run the preceding code snippet and plot the input/output images side-by-side, you should obtain a figure like the following one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_145_0.png)

*Figure 3.6 – Gamma correction using a LUT with PIL*

### With opencv-python

With OpenCV-Python, the same operation can be implemented using `cv2.LUT()`. The `γ_correct()` function in the next code first creates a 256-entry lookup table containing gamma-corrected intensity values and then applies it to the image:

```python
def γ_correct(img, γ):
    table = [((i / 255) ** γ) * 255 for i in range(256)]
    table = np.array(table, np.uint8)
    return cv2.LUT(img, table)

img = np.array(img)
plt.imshow(np.hstack((img, γ_correct(img, γ))))
plt.axis('off')
```

If you run the preceding code snippet, you should obtain the following figure:

original and γ-corrected image

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_146_0.png)

Figure 3.7 – Gamma correction using a LUT with opencv-python

Compare the implementation with opencv-python versus PIL and vary the gamma parameter to observe the impact on the output image.

## Simulating lens blur effects

Lens blur simulates the depth-of-field effect of a real camera, causing out-of-focus regions to appear smooth while keeping selected areas sharp. The implementation first applies gamma correction using `image.point()` with the transfer function `I' = I^p` (for example, `p = 0.5`) to enhance contrast before blurring. A blur effect is then generated with `ImageFilter.GaussianBlur(radius=5)`, which approximates out-of-focus lens behavior. Blur intensity is controlled spatially using a mask and `Image.composite()`, which blends the original and blurred images, preserving sharp regions where required. Finally, inverse gamma correction is applied to restore brightness, and multiple blur layers can be combined to produce a more natural depth-of-field appearance.

Now, refer to the following code snippet:

```python
from PIL import ImageFilter
from PIL.ImageChops import lighter

image = Image.open('images/me8.jpg').convert('RGB')
mask = Image.open('images/mask.png').convert('L')
p = 0.5
r = 5

gammaCorrectedImage = image.point(lambda x: x ** p)
bokeh = gammaCorrectedImage.filter( \
                         ImageFilter.GaussianBlur(radius=r))
bokeh = bokeh.point(lambda x: x ** (1/p)) # To get back the
                                          # original gamma.

bokeh = Image.composite(image, bokeh, mask)
blurImage = image.filter(ImageFilter.GaussianBlur(radius=r))
blurImage = Image.composite(image, blurImage, mask)
finalImage = lighter(bokeh, blurImage)
finalImage.show()
```

If you run the preceding code snippet, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_147_0.png)

*Figure 3.8 – Simulating lens blur effects with PIL*

Using $x^p$ ($0 < p < 1$) before blurring brightens shadow regions and improves blur quality, while applying the inverse transform $x^{\frac{1}{p}}$ afterward approximately restores the original brightness and contrast.

## Interchanging image color palettes

Palette-based images (**P mode**) store pixel values as indices into a **Color Lookup Table (CLUT)** rather than full RGB values, reducing memory and file size. The next code snippet swaps the adaptive 16-color palettes of two images, remapping their colors while preserving image structure.

First the images are converted to 16-color **adaptive palettes** using the function `convert("P", palette=Image.ADAPTIVE, colors=16)`, which performs adaptive color quantization while preserving visual appearance. The corresponding palettes are extracted with `getpalette()`, and the helper function `plot_palette()` visualizes them by reshaping the flat RGB palette list into a `(num_colors,3)` array and displaying it as a color swatch. The palettes are then exchanged using `putpalette()`, after which the images are saved with their new palettes. Although the pixel indices and image structures remain unchanged, the colors are remapped according to the swapped palettes, producing visually striking and artistic effects.

Refer to the following code snippet:

```python
def plot_palette(palette, num_colors=16, title='Palette'):
    plt.imshow([np.array(palette[:num_colors*3]).reshape(\
                (num_colors, 3)) / 255], aspect='auto')
    plt.show()

img1, img2 = Image.open('images/parrot.jpg'), Image.open('images/lena.jpg')
img1_p = img1.convert("P", palette=Image.ADAPTIVE, colors=16)
img1_p.save('images/parrot_adaptive16_orig.png')
img2_p = img2.convert("P", palette=Image.ADAPTIVE, colors=16)
img2_p.save('images/lena_adaptive16_orig.png')
print(os.stat('images/parrot.jpg').st_size,
      os.stat('images/parrot_adaptive16_orig.png').st_size)
data = list(img1_p.getdata())
print(data[50:70])
clrs = img1_p.getcolors()
print(len(clrs))

# show up colors and palette
pal1 = img1_p.getpalette()
pal2 = img2_p.getpalette()
plot_palette(pal1, title='Palette of Image 1 before swap')
plot_palette(pal2, title='Palette of Image 2 before swap')
```

Display the palettes using the preceding code snippet. You should obtain a figure like the following one:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_148_0.png)

*Figure 3.9 – Palettes of the images before swapping*

Now, swap the palettes, using the following code snippet:

```python
img1_p.putpalette(pal2); img2_p.putpalette(pal1)
```
