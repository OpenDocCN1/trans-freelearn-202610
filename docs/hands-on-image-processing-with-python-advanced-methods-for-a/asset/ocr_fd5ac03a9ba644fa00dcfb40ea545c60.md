The above figures show the input images and the output images generated with the preceding code snippet. The next figure shows the input, mask, and output images:

![](bbox=[0.254, 0.121, 0.796, 0.459])

*Figure 8.31 – The images to blend, the mask, and the output image with pyramid-blending*

This approach enables smooth transitions between image regions by blending at multiple spatial frequencies, effectively avoiding harsh edges that result from simple binary masking. Pyramid blending is particularly powerful for creating realistic composite images in applications like panorama stitching and object replacement.

# Choosing the right edge detector

A recurring challenge in edge detection is not implementation, but selection. Different operators respond to different image characteristics such as noise level, structural geometry (edges vs blobs vs ridges), and computational constraints. The following table provides a practical mapping from scenario to method, enabling informed decision-making:

| Scenario | Recommended method(s) | Rationale |
|---|---|---|
| Low-noise images, quick results | Sobel, Prewitt | Fast first-order gradients suffice |
| Noisy images | Canny, LoG | Built-in smoothing reduces noise sensitivity |
| Thin, well-localized edges required | Canny | Non-maximum suppression produces crisp edges |
| Blob-like structures (spots, lesions) | Hessian (DoH) | Detects structures with curvature in all directions |