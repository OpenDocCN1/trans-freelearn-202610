The optimal thresholds are obtained by solving:

(t₁*, …, tₖ₋₁*) = argmaxₜ₁,…,ₜₖ₋₁ ∑ᵢ₌₁ᴷ ωᵢ (μᵢ − μ_T)²

This is equivalent to maximizing **inter-class variance**, which implicitly minimizes **intra-class variance** due to:

σ²_total = σ²_within + σ²_between

# Computational considerations

A brute-force search over all threshold combinations has complexity:

O(L^{K−1}),

where L is number of gray-levels. The complexity increases exponentially with number of classes K. Practical implementations (such as in scikit-image) use dynamic programming or histogram precomputation to significantly reduce computational cost.

Multi-Otsu essentially models the histogram as a mixture of K clusters. It tries to find thresholds that best separate these clusters. It works best when histogram is multi-modal.

The next code snippet uses scikit-image implementation to demonstrate the Multi-Otsu algorithm. It computes optimal intensity thresholds based on the specified number of desired classes. By default, the number of classes is set to three, which results in two optimal threshold values partitioning the histogram into three distinct regions. These thresholds are visualized as vertical red lines on the intensity histogram in the figure below, clearly indicating the separation between classes.

The function threshold_multiotsu() from the module skimage.filters computes optimal thresholds.

Default behavior: K = 3 classes and returns K − 1 = 2 thresholds: [t₁, t₂]. Internally it computes histogram (p(i)), searches threshold combinations and maximizes between-class variance. The function np.digitize() assigns each pixel to a class:

Class 0: I ≤ t₁, Class 1: t₁ < I ≤ t₂ and Class 2: I > t₂.

**Histogram and Thresholds:** Histogram approximates the PMF p(i). Red lines = optimal thresholds and peaks correspond to different regions (e.g., background, object, highlights). Thresholds lie between peaks. Finally the segmented regions are displayed with pseudo-colors, each color corresponds to a class. This produces a label map, as shown:

```
from skimage import data
from skimage.filters import threshold_multiotsu
image = data.camera()
thresholds = threshold_multiotsu(image)
regions = np.digitize(image, bins=thresholds)
# Plot image, histogram (use image.ravel() to flatten image) and regions
```