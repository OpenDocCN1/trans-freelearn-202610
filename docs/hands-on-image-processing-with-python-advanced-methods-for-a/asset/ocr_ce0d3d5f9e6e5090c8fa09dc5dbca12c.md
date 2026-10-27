If you run the above code and plot the images side by side, you should obtain a figure like the following:

![](bbox=[0.275, 0.095, 0.77, 0.315])

Figure 8.4 – Input image convolved with the Laplacian operator

The above figure shows the output of the preceding code snippet. As can be seen, the Laplacian output also finds the edges in the image.

# Blur detection using the Laplacian variance

Blurring is one of the most fundamental spatial operations in image processing. It’s used both intentionally (e.g., for artistic effects or preprocessing) and unintentionally (e.g., due to poor focus or motion during capture). In many computer vision applications, detecting whether an image is blurred or not is a critical step—for instance, in quality assurance, autofocus systems, medical imaging, or preprocessing pipelines where high-frequency details are essential.

This section explores an efficient, no-reference (blind) technique for detecting blur in an image using the variance of the Laplacian operator. We will explain the mathematical intuition, apply common spatial filters (box and Gaussian), and present a well-commented Python implementation that demonstrates the technique step by step.

Blurring removes high-frequency content like edges and fine details. The Laplacian operator, being a second-order derivative, is sensitive to these sharp transitions. By applying the Laplacian and computing the variance of the resulting image, we can measure the level of detail:

+   - High variance → many edges → sharp image
+   - Low variance → few edges → blurry image

In 2D, the discrete Laplacian operator is typically represented as:

ℒ = [[0, 1, 0], [1, -4, 1], [0, 1, 0]]

It highlights regions of rapid intensity change.