
- **Defuzzification:** Convert the aggregated fuzzy set into a single crisp output (usually via the centroid method). Input **membership functions (MFs)** include the following:
- **Dark:** Trapezoidal MF defined over the lower range of intensity values
- **Medium:** Triangular MF centered around the mid-range
- **Bright:** Trapezoidal MF over the higher intensities

The dynamic range of the input image is first determined using the minimum and maximum pixel intensities. The fuzzy sets are then placed relative to this range. This ensures that the fuzzy system is adapted to the actual intensity span of the input image. Output MFs include the following:

- **Darker:** Designed to reduce intensity
- **Enhanced:** Targets midrange enhancement
- **Brighter:** Increases intensity values

The mapping from input to output is defined by Mamdani rules. The outputs are also defined using scikit-fuzzy membership functions (e.g., `trapmf`, `trimf`), and the same universe of discourse is used.

> The fuzzy system can be further improved by increasing the number of membership functions and defining more detailed rules. This allows for finer control and better adaptability to diverse image characteristics. The following implementation processes one pixel at a time for readability. On a typical machine, expect ~1 minute for a 512×512 image. For production use, consider vectorizing using `np.vectorize()` or replacing the loop with NumPy array operations.

Let’s now consider the following code snippet:

```
import skfuzzy as fuzz
def fuzzy_enhance_pixel(value):
    dark_level = fuzz.interp_membership(x, input_dark, value)
    medium_level = fuzz.interp_membership(x, input_medium, value)
    bright_level = fuzz.interp_membership(x, input_bright, value)
    rule_darker = np.fmin(dark_level, output_darker)
    rule_enhanced = np.fmin(medium_level, output_enhanced)
    rule_brighter = np.fmin(bright_level, output_brighter)
```

```
```python
aggregated = np.fmax(rule_darker, np.fmax(rule_enhanced,
    rule_brighter))
return fuzz.defuzz(x, aggregated, 'centroid')

def fuzzy_image_enhancement(img_gray):
    img_out = np.zeros_like(img_gray, dtype=np.uint8)
    h, w = img_gray.shape
    for i in range(h):
        for j in range(w):
            pixel = img_gray[i, j]
            enhanced_val = fuzzy_enhance_pixel(pixel)
            img_out[i, j] = np.clip(enhanced_val, 0, 255)
    return img_out

img_path = "images/cheetah.png"
img_gray = cv2.imread(img_path, cv2.IMREAD_GRAYSCALE)
img_min, img_max = int(np.min(img_gray)), int(np.max(img_gray))
x = np.arange(0, 256, 1) # fuzzy membership functions for input / output
range_width = img_max - img_min # fuzzy input MFs key points
low = img_min + 0.25 * range_width
mid = img_min + 0.5 * range_width
high = img_min + 0.75 * range_width
input_dark = fuzz.trapmf(x, [img_min, img_min, low, mid]) # Input fuzzy sets
input_medium = fuzz.trimf(x, [low, mid, high])
input_bright = fuzz.trapmf(x, [mid, high, img_max, img_max])
output_darker = fuzz.trapmf(x, [0, 0, 30, 100]) # Output fuzzy sets
output_enhanced = fuzz.trimf(x, [50, 128, 255])
output_brighter = fuzz.trapmf(x, [150, 200, 255, 255])
# Plot input and output membership functions here, refer to github notebook

img_fuzzy = fuzzy_image_enhancement(img_gray)
# Display result images (img_gray and img_fuzzy)
```
```

If you run the above code snippet, you should obtain the following figures:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_328_0.png)

*Figure 7.14 – Fuzzy membership functions*

The following figure shows the enhanced image obtained with Fuzzy HEQ, along with the original image side by side:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_328_1.png)

*Figure 7.15 – Fuzzy Histogram Equalization*

Let’s now try to understand how the above code snippet works:

- **Input/Output MFs**: Dynamically scaled based on the input image range.
- **Visualization**: Shows both MF plots and enhanced image output.
- `fuzzy_enhance_pixel`: Uses `fuzz.interp_membership()` to perform fuzzification by computing how strongly a pixel value belongs to each fuzzy set. Performs fuzzification, inference, aggregation, and defuzzification for a single pixel. It uses `np.fmin()` for Mamdani-style rule application (minimum between antecedent and consequent). It aggregates rule outputs using `np.fmax()`. It uses `fuzz.defuzz()` for centroid-based defuzzification.

- `fuzzy_image_enhancement`: Iterates over all pixels and applies enhancement.
- **Dynamic membership functions**: Input fuzzy sets are scaled based on image intensity range (img_min, img_max), making them adaptive.
  Output MFs remain fixed but can be modified to adjust contrast enhancement.
- **Mapping**: The mapping from input to output is achieved through predefined fuzzy rules. Each input region is associated with a specific transformation (darker, enhanced, or brighter) through corresponding output MFs.

Advantages of fuzzy contrast enhancement include the following:

- Adaptive to image characteristics (min/max)
- Preserves fine details
- Avoids over-enhancement and artifacts
- Human-understandable rules and reasoning

## Histogram matching

**Histogram matching** is a process where an image is altered in such a way that its histogram matches that of another reference (**template**) image’s histogram. Its use cases include the following:

- **Remote sensing**: Normalizing satellite imagery from different sensors
- **Medical imaging**: Standardizing scans taken under different conditions
- **Photography**: Matching aesthetics across photos taken with varying light or cameras

The algorithm is as follows:

1. The cumulative histogram is computed for each image, as shown in the following figure
2. For any given pixel value, $x_i$, in the input image (to be adjusted), we need to find the corresponding pixel value, $x_j$, in the output image by matching the input image’s histogram with the template image’s histogram
3. The $x_i$ pixel value has a cumulative histogram value given by $G(x_i)$. Find a pixel value, $x_j$, so that the cumulative distribution value in the reference image, namely $H(x_j)$, is equal to $G(x_i)$.
4. The input data value xi is replaced by $x_j$

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_330_0.png)

Figure 7.16 – Histogram matching

## With scikit-image

As explained, histogram matching adjusts the color distribution of one image (the source) to match that of another image (the reference), based on their histograms. This process is particularly useful for correcting lighting inconsistencies or achieving a consistent visual appearance across image datasets.

The next code snippet uses the match_histograms() function from the skimage.exposure module. The function match_histograms() matches the cumulative distribution functions (CDFs) of the source and reference images independently across RGB channels. Here, channel_axis=-1 tells the function that the last dimension in the image array represents the color channels. The transformation performed is:

p' = F_r^{-1}(F_s(p))

where F_s and F_r are the CDFs of the source and reference, respectively, and p is the pixel intensity in the source image. This ensures that the intensity values in the output image have a histogram similar to that of the reference. Now, refer to the next code snippet:

```
from skimage.exposure import match_histograms
# Read source and reference images
source = imread('images/me.png')[..., :3]      # Image to correct
reference = imread('images/lena.jpg')[..., :3]  # Image with desired color profile
# Match histograms across RGB channels
matched = match_histograms(source, reference, channel_axis=-1)
```

```
matched = np.clip(matched, 0, 255).astype(np.uint8)
# plot images (source, reference and matched) here
```

If you run the above code snippet and plot the images, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_331_0.png)

Figure 7.17 – Histogram matching with scikit-image

The functions histogram() and cumulative_distribution() from skimage.exposure module can be used to obtain the histogram and CDF of an image (refer to the GitHub notebook).

## Enhancements in colors

Color enhancement is a fundamental step in digital image processing used to improve visual quality, correct imbalances, or produce stylistic effects. Unlike brightness or contrast adjustments, which operate on pixel intensity, color enhancement focuses on adjusting the saturation and chromatic content of an image. Color, defined by combinations of red, green, and blue channels in RGB images, can be enhanced or muted to better align with human perception or visual intent. Enhancing colors can make an image look more vibrant, while desaturating it can evoke subtle, artistic moods.

Python’s PIL.ImageEnhance module offers a simple yet powerful interface for adjusting color saturation. Here is a quick example demonstrating color enhancement and desaturation:

```
img = Image.open("images/parrot.jpg")
enhancer = ImageEnhance.Color(img)
colorful = enhancer.enhance(1.8)
desaturated = enhancer.enhance(0.3)
plt.imshow(np.hstack((img, colorful, desaturated))), plt.axis('off')
```
