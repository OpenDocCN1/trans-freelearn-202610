Note
Benchmarking results shown here are based on CPU execution and may vary significantly depending on hardware, especially when using GPU-accelerated versions of these libraries (e.g., OpenCV with CUDA). For a fair comparison, ensure consistent hardware settings.

While performance is crucial when selecting an implementation, it’s equally important to understand the underlying mathematical operation, specifically, the difference between correlation and convolution, which are often confused but behave differently. Let’s explore this in the next section.

# Correlation versus convolution

Correlation and convolution both slide a kernel over an image to compute weighted sums, but convolution flips the kernel before applying it. While they are often similar in practice, the distinction is important for specific filtering tasks. Normalized cross-correlation, used for pattern matching, adjusts for brightness and contrast variations, ensuring consistent feature detection across images with different lighting conditions. The following equations describe the difference between correlation and convolution on an image, f:

+   - Correlation: g(x, y) = f ⋆ K = Σ_{u=-h}^{h} Σ_{v=-h}^{h} f(x + u, y + v) K(u, v)

+   - Convolution: g(x, y) = f ∗ K = Σ_{u=-h}^{h} Σ_{v=-h}^{h} f(x − u, y − v) K(u, v)

Example: Refer to the next figure for a step-by-step numeric example that shows how to compute the outputs of correlation and convolution using the preceding formulas, starting with a 4×4 input image, a 2×2 kernel, and using mode='valid'. We can slide the kernel directly over the input image and compute element-wise multiplications and sums, as shown here:

![](bbox=[0.22, 0.63, 0.8, 0.89])

Figure 5.8 – Compute correlation and convolution in 2D – a numeric example