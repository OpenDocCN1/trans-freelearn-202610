# Morphological operations

Morphological operations help **clean the binary mask and separate touching coins**. Let $B$ be the binary image and $S$ the SE (structuring element, a small, predefined shape/kernel used to probe and process image neighborhoods in morphological operations). Now, let’s define the morphological operations:

+   - **Erosion** ($\ominus$): $B \ominus S = \{z \mid S_z \subseteq B\}$: Removes small noise pixels and shrinks objects.
+   - **Dilation** ($\oplus$): $B \oplus S = \{z \mid S_z \cap B \neq \varnothing\}$: Expands objects and connects nearby pixels.
+   - **Opening** ($\circ$): $B \circ S = (B \ominus S) \oplus S$: Removes small objects/noise and smooths boundaries.
+   - **Closing** ($\bullet$): $B \bullet S = (B \oplus S) \ominus S$: Fills small holes inside objects.
+   - **Distance transform and watershed:** For separating touching coins first compute distance transform $D(x,y)$ of binary mask, where peaks correspond to coin centers. Use them as markers and apply **watershed algorithm** to split overlapping regions into distinct individual objects.
+   - **Labeling and counting:** After segmentation and morphological refinement, the binary mask contains **connected components**.

Label each component: $L = \operatorname{label}(M)$.

The **number of coins** is: $N_c = \max(L)$.

Let’s implement coin counting using the following code snippet. Here are the key steps:

+   1. **Load image:** The coin’s image using the function `coins()` from `skimage.data` module.
+   2. **Thresholding:** Use `filters.threshold_otsu()` function from `skimage` to separate foreground coins from background.
+   3. **Morphological cleaning:** Use the function `morphology.remove_small_objects()` from `skimage` and `binary_fill_holes()` from `scipy.ndimage` to remove noise and fill internal holes in coins, respectively.

```
```python
from skimage import io, color, filters, measure, morphology, data
gray = data.coins()
thresh = filters.threshold_otsu(gray)
binary = gray > thresh
cleaned = morphology.remove_small_objects(binary, min_size=100)
cleaned = ndi.binary_fill_holes(cleaned)
# plot gray, binary and cleaned images side-by-side here.
```
```