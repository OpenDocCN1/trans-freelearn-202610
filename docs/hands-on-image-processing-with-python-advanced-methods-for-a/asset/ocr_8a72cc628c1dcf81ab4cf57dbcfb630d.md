# Edge Detection using skimage.filters.rank.gradient_percentile

Upload an image and adjust the radius and percentile to visualize edge maps using percentile-based gradient detection.

![](bbox=[0.19, 0.09, 0.84, 0.34])

Figure 8.14 – Percentile-based gradient computation interactively with Gradio

The Gradio interface can be opened independently with a browser too (using the URL provided in the output). Play with the sliders to tune (change) input parameters for gradient-percentile-based edge computation and observe the impact on the output edges computed.

# The Canny edge detector with scikit-image

The Canny edge detector is a popular edge detection algorithm, developed by John F. Canny. This algorithm has the following multiple steps:

+   1. **Smoothing/noise reduction**: The edge detection operation is sensitive to noise. Hence, at the very outset, a 5 × 5 Gaussian filter is used to remove noise from the image.
+   2. **Computing magnitude and orientation of the gradient**: The Sobel horizontal and vertical filters are then applied to the image to compute the edge gradient magnitude and direction for each pixel, as discussed previously. The gradient angle (direction) computed is then rounded to one of four angles representing horizontal, vertical, and two diagonal directions for each pixel.
+   3. **Non-maximum suppression**: In this step, the edges are thinned – any unwanted pixels that may not constitute an edge are removed. To do this, every pixel is checked to see if it is a local maximum in its neighborhood in the direction of the gradient. As a result, a binary image is obtained with thin edges.
+   4. **Linking and hysteresis thresholding**: In this step, whether all the edges detected are strong edges or not is decided. For this, a couple of (hysteresis) threshold values, min_val and max_val, are used. Sure edges are the ones that have an intensity gradient value higher than max_val. Sure non-edges are the ones that have an intensity gradient value below min_val, and they are discarded. The edges that lie between these two thresholds are classified as edges or non-edges,