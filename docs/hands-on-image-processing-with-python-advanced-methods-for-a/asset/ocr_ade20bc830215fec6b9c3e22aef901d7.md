The modular architecture of the app makes it easy to extend and customize. Treat this app as a foundation for building more powerful and interactive image enhancement tools!

# Denoising with BM3D

Image denoising is a fundamental task in image processing aimed at recovering the original image from a noisy observation. **Block-Matching and 3D (BM3D)** filtering, proposed by Dabov et al. (2007), is a state-of-the-art denoising algorithm known for its ability to preserve edges and textures while effectively removing noise. This section provides a comprehensive explanation of BM3D, including its theory, mathematical formulation, and Python implementation.

Let the noisy image $y$ be modeled as $y = x + n$, where $x$ is the original clean image and $n$ is the additive white Gaussian noise (AWGN) with standard deviation (for in-depth discussion, refer to the image restoration chapter).

BM3D operates in two main stages:

+   - **Basic estimate** (hard thresholding): Groups similar patches, performs collaborative 3D filtering via hard-thresholding in the transform domain
+   - **Final estimate** (Wiener filtering): Uses the basic estimate to perform Wiener filtering for better accuracy

The overall BM3D denoising pipeline consists of three fundamental operations, namely block matching, collaborative filtering, and aggregation, which are performed iteratively during the two estimation stages described above:

+   - **Block matching:** Find groups of similar 2D patches {$P_i$} across the image. These patches are stacked into a 3D array: [$P_i^1$, $P_i^2$, ..., $P_i^k$].
+   - **Collaborative filtering:** Apply 3D linear transform (e.g., 2D DCT + 1D Haar). Perform thresholding or Wiener filtering. Inverse transform to get denoised patches.
+   - **Aggregation:** Overlap patches are aggregated using weighted averaging to form the final image.

To use BM3D in Python, let’s first install the bm3d library as shown in the next code snippet:

```
import bm3d  # first install with !pip install bm3d
image_path = 'images/mandrill_spnoise_0.3.jpg'  # Load a noisy image
noisy_image = img_as_float(io.imread(image_path))
denoised = bm3d.bm3d(noisy_image, sigma_psd=0.2, \
                    stage_arg=bm3d.BM3DStages.ALL_STAGES)
# Display the results here
```