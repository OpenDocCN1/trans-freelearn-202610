![](bbox=[0.13, 0.11, 0.85, 0.36])

> Figure 7.33 – Denoising with BM3D with different noise variances

BM3D provides state-of-the-art quality in denoising, preserves edges, fine details, and textures, and handles high and low noise levels effectively. However, it is computationally intensive compared to simpler methods and requires accurate noise level estimation.

# Wavelet-based image fusion and enhancement

Image fusion combines multiple images of the same scene to produce a single enhanced image containing more meaningful information. It is particularly useful in applications such as medical imaging, remote sensing, and exposure fusion in photography. Wavelet transform-based image fusion is a widely used multi-resolution approach that decomposes the input images into frequency sub-bands and combines them using fusion rules. The inverse transform is then used to reconstruct the fused image. This section provides a detailed understanding of wavelet-based image fusion along with mathematical explanation, intuition, and a Python implementation.

# Discrete wavelet transform

The discrete wavelet transform (DWT) decomposes an image into approximation and detail coefficients:

+   - Approximation coefficients (LL): Low-frequency components
+   - Detail coefficients (LH, HL, HH): High-frequency components in horizontal, vertical, and diagonal directions, respectively