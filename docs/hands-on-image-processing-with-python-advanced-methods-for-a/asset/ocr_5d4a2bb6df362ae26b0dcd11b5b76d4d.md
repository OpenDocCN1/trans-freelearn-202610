# ## Variance thresholding

Let $\mathcal{L}(x,y)$ be the Laplacian-transformed image. Then the **variance of the Laplacian** is computed as:

$$Var_{\mathcal{L}} = \frac{1}{N}\sum_{i=1}^{N}(\mathcal{L}_i-\mu)^2$$

Where:

+   - $N$ is the number of pixels
+   - $\mathcal{L}_i$ are Laplacian pixel values
+   - $\mu$ is the mean of $\mathcal{L}$

Set a threshold $T$ such that:

**Blurry** $\Longleftrightarrow Var_{\mathcal{L}} < T$

The threshold $T$ depends on image size, content, and application. A commonly used heuristic is between 50 and 150.

```
```python
def variance_of_laplacian(image):
    lap = cv2.Laplacian(image, cv2.CV_64F)
    return lap.var()


def apply_box_blur(image, ksize=15):
    return cv2.blur(image, (ksize, ksize))


def apply_gaussian_blur(image, ksize=15, sigma=5):
    return cv2.GaussianBlur(image, (ksize, ksize), sigma)


def detect_blur(image, threshold=100.0):
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    var_lap = variance_of_laplacian(gray)
    is_blurry = var_lap < threshold
    return is_blurry, var_lap

# Load and resize image
img_path = 'images/parrot.jpg'
original = cv2.imread(img_path)
original = cv2.resize(original, (400, 400))
# Apply filters
blur_box = apply_box_blur(original, ksize=15)
blur_gaussian = apply_gaussian_blur(original, ksize=15, sigma=5)
# Create image dictionary
images = {
```
```