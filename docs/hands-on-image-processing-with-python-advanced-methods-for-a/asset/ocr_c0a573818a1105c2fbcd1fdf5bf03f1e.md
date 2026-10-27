```
res = minimize_scalar(objective, bounds=(0.1, 0.75), method='bounded')
optimal_gamma = res.x
enhanced_img = enhance_with_gamma(img_norm, optimal_gamma)
# Convert to 8-bit image
img_out = (enhanced_img * 255).astype(np.uint8)
entropy_map = entropy(img_out, disk(5))
# Visualization: plot img, img_out and entropy_map here
```

If you run the above code and plot the above images side by side, you should obtain a figure like the following:

![](bbox=[0.08, 0.28, 0.85, 0.52])

Figure 8.7 – Entropy maximization-based Gamma Correction

The following are the observations:

+   - For low-contrast or dark images, the optimal γ is often less than 1, brightening the image and increasing entropy
+   - The enhancement is content-adaptive, driven by the intrinsic information content of the image
+   - The local entropy map highlights regions with complex textures or edges

As can be seen from above, the entropy maximization results in a γ value that achieves considerable contrast enhancement of the above low-contrast input image.

# Edge detection using derivatives

Let’s now explore classical edge detection methods that rely on derivatives, including Sobel, Prewitt, Roberts, Scharr, Canny, and Laplacian of Gaussian filters. Compass and percentile gradient techniques are also introduced for directional and local edge detection.

# Sobel edge detector with scikit-image

The (first-order) derivatives can be approximated better than using the finite difference. The Sobel operators shown in the following figure are used quite frequently: