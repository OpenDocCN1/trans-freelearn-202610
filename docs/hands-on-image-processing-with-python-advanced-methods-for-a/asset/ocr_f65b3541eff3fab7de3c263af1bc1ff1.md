+   3. **Superpixel segmentation (SLIC + Graph Cut):** Use the function `slic()` from `skimage.segmentation` to generates superpixels first and then build RAG and use *normalized cut* to minimize: `cut(A,B)/assoc(A,V)`, to produce segmentation S₂.

+   4. **Combine segmentations:** Use the function `join_segmentations()` to compute S = S₁ ∩ S₂, it keeps agreement between methods, as shown in the next code snippet:

```
from skimage.segmentation import slic, join_segmentations, watershed
coins = (255*rgb2gray(data.astronaut())).astype('uint8')
edges, markers = sobel(coins), np.zeros_like(coins)
foreground, background = 1, 2
markers[coins < 30.0] = background
markers[coins > 150.0] = foreground
ws = watershed(edges, markers)
seg1 = label(ws == foreground)
seg2 = slic(coins, n_segments=200, max_num_iter=160, \
            sigma=1, compactness=0.75, channel_axis=None, start_label=0)
g = graph.rag_mean_color(coins, seg2, mode='similarity')
seg2 = graph.cut_normalized(seg2, g)
segj = join_segmentations(seg1, seg2)
# Show the segmentations: plot coins, seg1, seg2 and segj side-by-side
```

![](bbox=[0.334, 0.489, 0.714, 0.754])

*Figure 10.16 – Ensemble segmentation*

As discussed above, learning-based classical segmentation uses handcrafted features and machine learning models to perform pixel-wise classification, while ensemble methods combine multiple segmentations for robust and accurate results.

We’ve so far learnt classical segmentation techniques, including thresholding, watershed and morphology-based methods, superpixels, variational models, graphical and probabilistic approaches, and learning-based classical segmentation. Building on these foundations, next we will explore modern