The next code snippet trains a MANet on multi-source satellite imagery patches to perform multi-class land-cover segmentation using weighted cross-entropy and Dice loss with data augmentation and patch-wise inference.

# Data preparation and training strategies

The following implementation pipeline addresses these challenges through patch-based training, label encoding, class-balancing strategies, and data augmentation to enable robust segmentation of large, heterogeneous satellite imagery:

+   1. **Patch-based training:** To overcome memory limitations, the large satellite images are divided into smaller patches (PATCH=512). The function `extract_patches()` extracts non-overlapping patches of size 512 × 512 from both the RGB image and the segmentation mask.

Mathematically,

$$P_{ij}=I[x_i:x_i+P, y_j:y_j+P],$$

where P = 512 denotes the patch size. This transforms the training dataset into thousands of smaller samples (Total patches: 5318). Patch-based training offers several advantages, such as reduced GPU memory consumption, increased dataset size, improved stochastic diversity, and easier mini-batch optimization.

+   2. **Color-Based Mask Encoding:** The dataset stores semantic masks as RGB color images rather than integer labels. Example:

| RGB Color | Semantic Class |
|---|---|
| (0,255,0) | Vegetation |
| (255,0,0) | Buildings |
| (0,0,255) | Water |

The next code snippet builds a mapping (`COLOR_MAP`).The encoding operation converts RGB masks into categorical label tensors:

$$M(x,y) \in \{0,1,\ldots,C-1\}$$

The conversion is performed using: `label[(mask==c).all(axis=-1)] = i`. This produces integer-valued segmentation masks suitable for neural network training.

+   3. **Class-imbalance compensation:** Class imbalance is addressed using median-frequency balancing.

Let $f_c = \frac{N_c}{\sum_k N_k}$ denote the frequency of class $C$, where $N_c$ is the number of pixels belonging to class $C$.

The class weight is defined as:

$$w_c = \frac{\operatorname{median}(f)}{f_c + \epsilon}$$