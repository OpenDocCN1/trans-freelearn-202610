
## Power-law transform

As we have already seen, this point transform (the transfer function) is of the general form:

\[
s = T(r) = c r^\gamma
\]

where \(c\) is a constant. After applying it on a grayscale image using the `PIL.Image.point()` method in Chapter 1, let’s apply a power-law transform on an RGB color image with scikit-image this time, and then visualize the impact of the transform on the color channel histograms.

```python
im = img_as_float(imread('images/cheetah.png'))
γ = 2.5 # try different values here and observe the impact
im_trans = im**γ
```

If you run the above code snippet and plot the images and corresponding RGB channel histograms, you should obtain the following figure, which shows the color channel histograms before and after the power-law transform, along with the original and transformed images:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_308_0.png)

Let pixel values be normalized in the range \([0,1]\). The effect of \(\gamma\) depends critically on whether it is less than or greater than 1 (as shown in the next table and the figure).

- When \(\gamma < 1\), the transformation expands lower intensity values, making dark regions brighter and improving visibility in shadows.
- Conversely, when \(\gamma > 1\), the transformation compresses lower intensities, causing the image to appear darker overall.

| Gamma | Curve shape | Effect |
|---|---|---|
| \(\gamma < 1\) | Above diagonal | Brightens shadows |
| \(\gamma = 1\) | Straight line | No change |
| \(\gamma > 1\) | Below diagonal | Darkens image |

Therefore, contrary to common intuition, increasing \(\gamma\) does not brighten the image; it darkens it, while values less than 1 produce a brightening effect. The transformation is nonlinear: it redistributes contrast, not just brightness.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_309_0.png)

*Figure 7.4 – Power-law transformation*

## Brightness adjustment

Brightness is one of the most fundamental image attributes. Adjusting it plays a critical role in image enhancement, preprocessing, visual aesthetics, and readability. In many real-world applications, especially those involving low-light, overexposed, or underexposed images, brightness correction is the first step toward better image interpretation or downstream processing (e.g., object detection, OCR, or classification). Let us explore how to adjust image brightness using libraries like PIL and opencv-python.

In digital imaging, brightness refers to the perceived intensity of an image. For grayscale images, it corresponds directly to pixel values. For color images, it’s more nuanced and often linked to the luminance or value component in a specific color space (e.g., HSV, Lab, or YUV). For an image \(I(x, y)\), the brightness-adjusted image \(I'(x, y)\) is typically computed by:

\[
I'(x, y) = \alpha \cdot I(x, y) + \beta
\]

where \(\alpha\) is a contrast scaling factor (often 1 for brightness-only adjustments) and \(\beta\) is a brightness offset (positive brightens, negative darkens).

Since digital images are represented with fixed value ranges (e.g., 0–255 for 8-bit), values after adjustment must be clipped:

\[
I'(x,y)=\min(255,\max(0,\alpha\cdot I(x,y)+\beta))
\]

The next table lists a few use cases where brightness adjustment turns out to be very useful:

| Domain | Purpose |
|---|---|
| Photography | Correct under-/overexposed shots |
| OCR preprocessing | Improve text visibility |
| Surveillance | Enhance dim or nighttime footage |
| Satellite imagery | Improve contrast before analysis |
| Medical imaging | Reveal hidden structures in scans |

*Table 7.2 – Brightness adjustment in different scenarios*

### With PIL

Let’s first explore how to adjust brightness using PIL with the next code snippet, following these steps:

1. First, load an image using `Image.open()` and create a brightness enhancer with `ImageEnhance.Brightness(img)`.
2. The method `enhance(1.5)` increases the brightness by 50% (where 1.0 is the original brightness).
3. Use matplotlib to display the original and brightened images side by side for comparison.

Essentially, it demonstrates how to quickly adjust image brightness using PIL and visualize the result. Now refer to the next code snippet:

```python
img = Image.open('images/boy.png')
enhancer = ImageEnhance.Brightness(img)
bright_img = enhancer.enhance(1.5)
dark_img = enhancer.enhance(0.5)
# plot images and intensity histogram here, refer to github code
```

If you run the above code and plot the images and channel histograms side by side, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_311_0.png)

*Figure 7.5 – Changing brightness of an image with PIL*

It has the following key advantages: it’s intuitive to use, supports all image modes, and includes built-in tools for gamma and color adjustments.

### With OpenCV-Python

Now let’s explore how to perform brightness adjustment using an affine transformation with opencv-python, using the following code snippet. Let’s understand the code with the following step-by-step explanation:

- First load the input image using `cv2.imread()` in BGR format. Convert it to RGB format using the function `cv2.cvtColor()` to correctly display with matplotlib.
- Apply an affine transformation to each pixel intensity with the function `cv2.convertScaleAbs()`:
  \[
  I'(x,y) = \alpha \cdot I(x,y) + \beta
  \]
- \(\alpha\) controls contrast:
  - \(\alpha > 1\): increases contrast (stretches intensity range)
  - \(0 < \alpha < 1\): decreases contrast (compresses intensity range)
  - \(\alpha = 1.0\): retains the original contrast

```python
high_contrast_img = cv2.convertScaleAbs(img, alpha=2, beta=0)
# Brightness increase by adding constant
high_brightness_img = cv2.convertScaleAbs(img, alpha=1.0, beta=50)
plt.imshow(np.hstack((img, high_contrast_img, high_brightness_img)))
plt.axis('off')
```

If you run the above code snippet, you should obtain the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_313_0.png)

*Figure 7.6 – Increasing brightness of an image with opencv-python*

This method is simple, efficient, and commonly used for real-time image enhancement. It supports both brightness and contrast via alpha and beta, is efficient for batch processing, and is used widely in computer vision pipelines.

## Tone and perceptual enhancement

This section explores intensity-based techniques, such as thresholding, halftoning, and dithering, that modify pixel tones to enhance visual perception or prepare images for limited display and printing devices, including the widely used Floyd–Steinberg error diffusion method.

### Thresholding

This is a point operation that creates binary images from gray-level ones by turning all pixels below some threshold to zero and all pixels above that threshold to one. If \(g\) is a thresholded version of \(f\) at some global threshold \(T\), then it can be expressed as:

\[
g(x,y)=\begin{cases}1, & f(x,y)>T\\0, & \text{otherwise}\end{cases}
\]

Why do we need a binary image? A few reasons are, for example, that we may be interested in separating an image into foreground and background; that the image is to be printed with a black and white printer (and all the shades of gray need to be represented using black and white dots only); or that we want to pre-process the image with **morphological operations**, as we shall discuss in a later chapter.

The following code snippet shows how to use the `PIL.Image.point()` method for thresholding with a fixed threshold:

```python
im = Image.open('images/swans.jpg').convert('L')
threshold = [0, 50, 100, 150, 200]
for i in range(2, 5):
    im_bin = im.point(lambda x: x > threshold[i])
    # plot the binary image obtained with this threshold value here
```

If you run the above code snippet and visualize the binary images obtained at different threshold values, you will notice how the choice of threshold affects the final result. First, observe how pixel intensities are distributed in the original image. When a single fixed threshold is used, all pixels are forced into just two categories: black or white.

This often causes a loss of smooth transitions present in the original image. As a result, regions that should change gradually instead appear as unnatural, sharp boundaries or band-like patterns. These artificial bands are referred to as *false contours* (they occur because continuous gray levels are abruptly split into only two levels, destroying smooth intensity variations). In simpler terms, instead of smooth shading, the image looks “stepped” or “posterized,” which reduces visual quality and may hide important details.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_314_0.png)

*Figure 7.7 – Creating binary images with different threshold values on the gray level*
