
If you run the above code and plot the images, you should get a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_490_0.png)

Figure 10.3 – Mathematical morphology to create binary mask from coins image

Here's a breakdown of what's happening:

+   1. **Distance transform:** Use the function distance_transform_edt() from the module scipy.ndimage to compute the distance transform output D (distance), here peaks correspond to coin centers.
+   2. **Marker extraction:** Use the function peak_local_max() from skimage.features module to detect local maxima in distance map computed above. These act as markers for watershed.
+   3. **Marker image construction:** Assign unique label to each detected peak.
+   4. **Watershed segmentation:** Use the function watershed() from skimage.segmentation to apply watershed segmentation on −D(x,y) so that peaks become basins and watershed separates touching coins.
+   5. **Visualization:** Plot the distance transform output (highlights centers), the marker map image (seeds for segmentation) and the watershed segmentation (separated coins) output side-by-side, as shown in the next code snippet:

```
from skimage.feature import peak_local_max
from skimage.segmentation import watershed

distance = ndi.distance_transform_edt(cleaned)
coords = peak_local_max(distance, footprint=np.ones((3, 3)),
labels=cleaned)
markers = np.zeros_like(distance, dtype=int)
for i, (r, c) in enumerate(coords, start=1): markers[r, c] = i
labels = watershed(-distance, markers, mask=cleaned)
# plot the images distance, markers and labels side-by-side here
```

If you run the above code and plot the images, you should get a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_491_0.png)

Here’s a breakdown of what’s happening in the code:

+   1. **Region-based filtering:** The function `skimage.measure.regionprops()` is used to compute the following properties of the segmented regions: area, perimeter, eccentricity, and solidity.
+   
+   2. **Circularity measure:**
+   
+      $$C = \frac{4\pi A}{P^2},$$
+   
+      where $A$ and $P$ are the area (total number of pixels) and perimeter of the corresponding region, respectively ($C \approx 1 \to$ circle). Use it to filter non-coin regions using a threshold, e.g., 0.7, as used in the code (a coin is supposed to have a higher circularity value than for example, a rectangular region with same perimeter). It removes noise and irregular shapes.
+   
+   3. **Final counting:** The function `skimage.measure.label()` is used to label refined regions. Then count the number of connected components to find the number of coins.
+   
+   4. **Visualization:** Plot the watershed labels image, cleaned labels image with regionprop filters and overlay image with coins with the center locations marked (obtained with the centroid attribute for each region, as the geometric center of a connected component:
+   
+      $$(x_c,y_c)=\left(\frac{1}{A}\Sigma x,\frac{1}{A}\Sigma y\right)$$
+   
+      side-by-side, as shown in the next code snippet:

```
```python
regions = measure.regionprops(labels)
coin_labels = []
for region in regions:
    circularity = 4. * np.pi * region.area / (region.perimeter ** 2 + 1e-6)
    if circularity > 0.7 and region.area > MIN_AREA:
        coin_labels.append(region.label)
coin_mask = np.isin(labels, coin_labels)
clean_labels = measure.label(coin_mask)
num_coins = clean_labels.max()
print(f"Number of coins detected: {num_coins}")
# Number of coins detected: 24
# Plot labels, clean_labels, gray side-by-side and mark centroid here
```
```

While SLIC generates superpixels based solely on color and spatial proximity, MaskSLIC extends this framework by incorporating prior mask information to constrain superpixel formation within regions of interest.

- **Mathematical formulation:** SLIC performs a localized **k-means clustering** using a distance metric:
  
  $$D = \sqrt{d_c^2 + \left(\frac{m}{S}\right)^2 d_s^2},$$
  
  where $d_c = \sqrt{(L_i - L_j)^2 + (a_i - a_j)^2 + (b_i - b_j)^2}$ (color distance), $d_s = \sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}$ (spatial distance), $S$: expected superpixel size and $m$: compactness parameter.

- **Optimization objective:** SLIC minimizes:
  
  $$\sum_{k=1}^{K} \sum_{i \in S_k} D\left(i, C_k\right)^2$$
  
  where $C_k$ is the cluster center.

- **MaskSLIC extension:** MaskSLIC introduces a constraint:
  
  $$S_k \subseteq \mathcal{M}$$
  
  where $\mathcal{M}$ is a predefined mask. The key idea is to restrict clustering to relevant regions, improve boundary adherence and avoid superpixels crossing object boundaries.

- **Region Adjacency Graph (RAG) merging:** Although *superpixels* significantly reduce computational complexity by grouping similar pixels, they typically produce an *over-segmented* representation in which a single semantic object may be fragmented into many small regions. To obtain larger and more meaningful object-level segments, adjacent superpixels with similar appearance are subsequently merged using a Region Adjacency Graph (RAG).
  
  After superpixelation, the image is represented as a **graph over regions** $G = (V, E)$, where $V = \{S_1, S_2, ..., S_K\}$ is the set of superpixels and $E$ is the set of edges between **adjacent regions**.

- **Node attributes (Region Statistics):** Each region $S_i$ is associated with features (e.g., mean color),
  
  $$\mu_i = \frac{1}{|S_i|} \sum_{(x,y) \in S_i} I(x, y),$$
  
  where $|S_i|$: number of pixels in region and $I(x, y)$: pixel intensity (or vector in RGB/Lab space).

- **Edge weights (Similarity Measure):** Edges encode **dissimilarity** between neighboring regions $w_{ij} = \| \mu_i - \mu_j \|_2$, s.t. small $w_{ij} \rightarrow$ similar regions; large $w_{ij} \rightarrow$ dissimilar regions.

- **Hierarchical region merging:** RAG merging performs **iterative agglomerative clustering**
  
  $$(S_i, S_j) = \arg \min_{(i,j) \in E} w_{ij}$$
  
  subject to $w_{ij} < \tau$, where $\tau$ is a threshold (Stop when no edges satisfy threshold).

When two regions $S_i$ and $S_j$ are merged, $S_{ij}=S_i\cup S_j$, updated mean

$$\mu_{ij}=\frac{|S_i|\mu_i+|S_j|\mu_j}{|S_i|+|S_j|}.$$

After merging, edge weights are recomputed $w_{(i,j),k}=\lVert\mu_{ij}-\mu_k\rVert_2$.

This is implemented via the function `_weight_mean_color()` (refer github). RAG merging minimizes intra-region variance

$$\min\sum_k\sum_{(x,y)\in S_k}\lVert I(x,y)-\mu_k\rVert^2,$$

subject to adjacency constraints.

- **RAG merging transforms:** Superpixels → Semantically meaningful regions by progressively grouping similar neighboring regions, forming a bridge between low-level oversegmentation and high-level segmentation.

The next code demonstrates and compares **SLIC** with **MaskSLIC**. First the input image is read and converted to *Lab* space using `rgb2lab()` for *perceptually uniform* clustering, while the mask is thresholded and refined using `remove_small_objects()`, `remove_small_holes()`, and `opening()`.

The function `segmentation.slic()` generates SLIC superpixels in $(x,y,L,a,b)$ feature space, whereas MaskSLIC restricts segmentation to masked regions for more meaningful superpixels. RAG-based merging is then applied to both segmentations to obtain semantically coherent regions.

The helper functions (`_weight_mean_color()`, `merge_mean_color()`, `merge_segments()`, refer github) construct a RAG using `graph.rag_mean_color()` and hierarchically *merge adjacent regions* based on mean color similarity via `graph.merge_hierarchical()`.

Finally, `mark_boundaries()` visualizes the original image, mask, SLIC, MaskSLIC, and their merged outputs side-by-side.

```
```python
def merge_segments(img, segments):
    rag = graph.rag_mean_color(img, segments)
    labels = graph.merge_hierarchical(
        segments, rag, thresh=35,
        rag_copy=False, in_place_merge=True,
        merge_func=merge_mean_color,
        weight_func=_weight_mean_color)
    return color.label2rgb(labels, img, kind='avg')

img = plt.imread('images/cats.jpg') #input data
lum = color.rgb2lab(img)
mask = color.rgb2gray(plt.imread('images/cat_mask.jpg')) > 0.1
mask = morphology.remove_small_objects(mask, min_size=499)
mask = morphology.remove_small_holes(mask, area_threshold =499)
mask = morphology.opening(mask, morphology.disk(3))
slic = segmentation.slic(lum, n_segments=200, start_label=1)
```
