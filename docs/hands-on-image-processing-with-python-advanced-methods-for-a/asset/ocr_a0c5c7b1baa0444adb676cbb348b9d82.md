# # Image Restoration: Inverse Problems in Imaging

Image restoration is a fundamental problem in image processing and computer vision. Its primary goal is to reconstruct or recover a high-quality image from an observed, degraded version. Degradations may arise due to limitations in imaging systems, environmental conditions, or transmission channels, and often manifest as blur, noise, low resolution, or missing data. The ability to restore images has immense practical importance, influencing domains such as medical diagnostics, astronomy, remote sensing, photography, and surveillance.

Unlike **image enhancement**, which subjectively improves visual quality without necessarily modeling the source of degradation, **image restoration** relies on a mathematical model of the degradation process. By incorporating knowledge of how the original image was altered, restoration methods aim to “invert” the process and approximate the true, undegraded image.

In this chapter, we will study a progression of techniques for image restoration, beginning with classical analytical methods, moving through sparse representations, and finally arriving at modern deep learning-based approaches. Specifically, we will cover the following key topics:

+   - Image restoration as an inverse problem
+   - Classical approaches to image deconvolution
+   - Image restoration with sparse representations
+   - Image restoration with deep learning
+   - Stable Diffusion-based inpainting with DeepFill