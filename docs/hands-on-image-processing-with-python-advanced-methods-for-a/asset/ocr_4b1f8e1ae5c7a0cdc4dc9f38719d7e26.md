mum). Pixels “shift” toward high-density regions, grouping into segments without specifying the number of clusters.

Use functions watershed() and slic() to implement watershed segmentation and SLIC superpixelation, respectively, as shown in the next code snippet:

```
from skimage.segmentation import felzenszwalb, slic, quickshift, \
    watershed, mark_boundaries

img = plt.imread('images/rose.jpg')[:, ::2, ::2] # downsamples
segments_fz = felzenszwalb(img, scale=100, sigma=0.5, min_size=50)
segments_slic = slic(img, n_segments=250, compactness=10, sigma=1, \
                    start_label=1)

segments_quick = quickshift(img, kernel_size=3, max_dist=6, ratio=0.5)
gradient = sobel(rgb2gray(img))
segments_watershed = watershed(gradient, markers=250, compactness=0.001)
fz_merged = merge_segments(img, segments_fz)
slic_merged = merge_segments(img, segments_slic)
quick_merged = merge_segments(img, segments_quick)
watershed_merged = merge_segments(img, segments_watershed)
# PLOT the segmented and rag-merged images here
```

If you run the above code and plot the images, you should get a figure like the following:

![](bbox=[0.18, 0.48, 0.85, 0.7])

Figure 10.7 – Segmentation (Quickshift, Watershed, Felzenszwalb) vs. Superpixelation (SLIC)

Superpixelation is not final segmentation, it acts as a mid-level representation. It enables efficient graph-based learning and region pooling in deep networks. Superpixel-based segmentation provides a powerful abstraction: SLIC clusters pixels in 5D feature space. MaskSLIC incorporates prior masks for precision. Multiple algorithms offer trade-offs. SLIC Combined with RAG: Pixels → Superpixels → Regions. This hierarchical view forms a crucial bridge between classical segmentation and modern deep learning pipelines, enabling both efficiency and interpretability.