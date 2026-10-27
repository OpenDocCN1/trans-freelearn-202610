# Why zero-mean patches?

Many dictionary learning pipelines remove the DC (mean) from each patch before training. This decouples low-frequency (mean/brightness) from texture information and lets atoms focus on structure (edges, patterns). Re-adding the patch mean after reconstruction preserves the correct brightness.

The next code snippet implements a patch-based dictionary-learning denoising. Here are the steps:

+   - Load a grayscale image and add Gaussian noise.
+   - Extract overlapping patches from the noisy image (and from the clean image for dictionary training).
+   - Learn a dictionary $D$ from clean patches (subset).
+   - Zero-mean patches and use OMP to compute sparse codes for noisy patches.
+   - Reconstruct patches $\hat{x}_i = D\hat{\alpha}_i$ and re-add the patch means.
+   - Recompose the image by averaging overlapping patch reconstructions.

This is a classic pipeline (Elad & Aharon style) that removes noise by projecting local patches onto the learned low-dimensional manifold.

Let us go through the code step by step:

+   1. **Define parameters:** The next code block defines the following parameters:
+      - `patch_size`: Smaller patches (6–12) capture local texture; 8×8 is standard.
+      - `stride`: Smaller stride → more overlap (better quality, slower).
+      - `n_atoms`: must be larger than patch dimensionality / redundancy helps; $p^2=64$ so 512 is overcomplete ×8.
+      - `sparsity`: Typically 2–6 for 8×8 patches; controls denoising strength.
+      - `n_samples_for_dict`: Number of training patches to learn dictionary; large for robust atoms.

Now refer to the following code:

```
patch_size = 8          # p (patch size)
stride = 3              # step between patches (overlap)
n_atoms = 512            # K, dictionary size
sparsity = 4             # s, nonzeros per patch
n_samples_for_dict = 40000
random_state = 0
```

+   2. **Load and grayscale conversion:** Load the Lena image and convert to float in [0,1]; if RGB, use luminance conversion to grayscale, using the next code:

```
img = img_as_float(io.imread("images/lena.jpg"))
if img.ndim == 3:
    img_gray = color.rgb2gray(img)
else:
    img_gray = img.copy()
```