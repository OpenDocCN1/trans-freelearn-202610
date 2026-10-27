
If you run the preceding code snippet, you should obtain a figure like the following one.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_244_0.png)

*Figure 5.6 – Applying the sharpening filter with opencv-python*

## Performance benchmarking

The complete benchmarking code comparing `scipy.signal.convolve2d()`, `scipy.ndimage.convolve()`, and `cv2.filter2D()` is available in the GitHub repository. The benchmark evaluates runtime across different image sizes and shows that `cv2.filter2D()` generally provides the best performance due to its optimized implementation; refer to the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_244_1.png)

*Figure 5.7 – Benchmarking runtime of 2D convolution (scipy signal/ndimage vs opencv-python)*

Note
Benchmarking results shown here are based on CPU execution and may vary significantly depending on hardware, especially when using GPU-accelerated versions of these libraries (e.g., OpenCV with CUDA). For a fair comparison, ensure consistent hardware settings.

While performance is crucial when selecting an implementation, it’s equally important to understand the underlying mathematical operation, specifically, the difference between correlation and convolution, which are often confused but behave differently. Let’s explore this in the next section.

## Correlation versus convolution

Correlation and convolution both slide a kernel over an image to compute weighted sums, but convolution flips the kernel before applying it. While they are often similar in practice, the distinction is important for specific filtering tasks. Normalized cross-correlation, used for pattern matching, adjusts for brightness and contrast variations, ensuring consistent feature detection across images with different lighting conditions. The following equations describe the difference between correlation and convolution on an image, f:

- Correlation: g(x, y) = f ⋆ K = Σ_{u=-h}^{h} Σ_{v=-h}^{h} f(x + u, y + v) K(u, v)
- Convolution: g(x, y) = f ∗ K = Σ_{u=-h}^{h} Σ_{v=-h}^{h} f(x − u, y − v) K(u, v)

Example: Refer to the next figure for a step-by-step numeric example that shows how to compute the outputs of correlation and convolution using the preceding formulas, starting with a 4×4 input image, a 2×2 kernel, and using mode='valid'. We can slide the kernel directly over the input image and compute element-wise multiplications and sums, as shown here:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_245_0.png)

Figure 5.8 – Compute correlation and convolution in 2D – a numeric example

Hence, correlation and convolution differ in whether the kernel is flipped before application. The next figure summarizes how this affects the output:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_246_0.png)

Figure 5.9 – 2D correlation versus convolution – a numeric example

The scipy.signal module's correlate2d() function can be used for correlation. The following code can be used to verify the preceding manual computation, by visually demonstrating the difference between correlation and convolution outputs using annotated heatmaps using the seaborn library:

```
from scipy.signal import convolve2d, correlate2d
import seaborn as sns
k = np.reshape(range(1,5), (2,2))
I = np.reshape(range(1,17), (4,4))
conv_out = convolve2d(I, k, mode='valid')
corr_out = correlate2d(I, k, mode='valid')
sns.heatmap(conv_out, annot=True, fmt='d', linewidths=.5, cbar=False)
sns.heatmap(k, annot=True, fmt='valid', linewidths=.5, cbar=False)
```

You should obtain a plot like the following one if you run the preceding code.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_246_1.png)

Figure 5.10 – 2D correlation versus convolution output

Correlation is identical to convolution when the kernel is symmetric. But if the kernel is not symmetric (e.g., an asymmetric 3 × 3 ripple kernel [0,-1,√2], [1,0,-1], [-√2,1,0]), to get identical results with `correlation2d()` and `convolution2d()`, the kernel must be flipped both vertically and horizontally before applying correlation to the image. Let’s move on to see an interesting application of correlation.

## Template matching

Template matching is a technique used in image processing and computer vision to find a small template image, *T*, within a larger image, *I*. It is commonly used for object detection, pattern recognition, and feature localization.

### Using cross-correlation

For a given image, *I(x, y)*, and a template, *T(i, j)*, of size *M × N*, the cross-correlation at position *(x, y)* is computed as follows:

$$C(x,y)=\sum_{i=0}^{M-1}\sum_{j=0}^{N-1} I(x+i,y+j)\cdot T(i,j)$$

Where *I(x + i, y + j)* is the image pixel at position *(x + i, y + j)* and *T(i, j)* is the corresponding template pixel. The sum is taken over all pixels in the template. This formula measures the similarity between the template and the corresponding region of the image. Higher values of *C(x, y)* indicate stronger matches.

### With scipy.signal

The next code demonstrates **template matching** using **zero-mean cross-correlation** with `scipy.signal.correlate2d()` to locate the right eye in a grayscale raccoon image. The face image and eye template are first converted to *zero-mean* by subtracting their mean intensities, improving matching robustness. Gaussian noise is then added to simulate real-world conditions. The cross-correlation is computed using `signal.correlate2d(face_image, template_image, boundary='symm', mode='same')`, where `boundary='symm'` applies symmetric padding and `mode='same'` preserves the image size. The location of the best match is obtained with `np.argmax()` and `np.unravel_index()`, which identify the coordinates of the maximum correlation value, and the detected template location is visualized by marking it on the image.

Here is the code snippet:

```
face_image = misc.face(gray=True) - misc.face(gray=True).mean()
template_image = np.copy(face_image[300:365, 670:750]) #righteye
template_image -= template_image.mean()
# add random noise
face_image = face_image + \
                         np.random.randn(*face_image.shape) * 50
cc = signal.correlate2d(face_image, template_image, \
                         boundary='symm', mode='same')
# find the match
```

```
```python
y, x = np.unravel_index(np.argmax(cc), cc.shape)
# plot face_image, template_image, cc (use cmap='afmhot')
# mark the location (x,y) with a red dot in the image
```
```

If you run the preceding code and plot the images, you should obtain a figure like the one below:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_248_0.png)

*Figure 5.11 – Detecting the right eye of the raccoon using correlation*

As can be seen from the preceding figure, the right eye of the raccoon in the input image has the highest cross-correlation with the eye-template image.

> **Note**
> 
> Template matching is widely used in real-world applications such as facial landmark detection, where parts such as eyes or mouth corners are located using small templates, and industrial quality inspection, where components or defects are identified by comparing against predefined patterns. These practical uses highlight the importance of template matching in both consumer and industrial computer vision systems.

## Using normalized cross-correlation (NCC)

Cross-correlation measures the similarity between an image, *I*, and a template, *T*, but it is sensitive to variations in lighting and contrast. **Normalized cross-correlation (NCC)** overcomes this issue by normalizing both the image and template, ensuring the correlation values remain between [−1,1], making them scale- and illumination-invariant.

**Mathematical definition of NCC:** The normalized cross-correlation at position $(x,y)$ is given by the following:

$$NCC(x,y)=\frac{\sum_{i,j}(I(x+i,y+j)-\mu_I)(T(i,j)-\mu_T)}{\sqrt{\sum_{i,j}(I(x+i,y+j)-\mu_I)^2\sum_{i,j}(T(i,j)-\mu_T)^2}}$$

Where $\mu_I$ and $\mu_T$ are the mean values of the image patch and template, respectively. The numerator computes the zero-mean cross-correlation. The denominator normalizes by the standard deviations, ensuring scale invariance.
