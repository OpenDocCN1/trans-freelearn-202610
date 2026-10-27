# 8

# Image Enhancements Using Derivatives

This chapter explores derivative-based image enhancement, using gradients, Laplacians, and higher-order operators to highlight edges, sharpen details, and enhance local structures. We cover classical methods like Sobel, Canny, and Laplacian of a Gaussian (LoG), as well as compass and percentile-gradient techniques, ridge detection, structured edge detection, and deep learning approaches such as Holistically-Nested Edge Detection (HED) and Pixel Difference Network (PiDiNet). Multiscale enhancement with Gaussian and Laplacian pyramids is also discussed. Practical Python implementations with scikit-image, OpenCV, and scipy provide hands-on experience in applying these methods for edge detection, contrast enhancement, and detail refinement. To make the concepts hands-on, an interactive Gradio app demonstrates Canny edge detection, allowing readers to experiment with thresholds and visualize results in real time; note that other derivative-based enhancements are not included in the app.

The key topics to be covered in this chapter are as follows:

+   - Image derivatives
+   - Classical enhancement using derivatives
+   - Edge detection using derivatives
+   - Structured and ridge-based edge detection
+   - Deep learning-based edge detection
+   - Derivative-based multiscale enhancement

# Technical requirements

Follow along with the Jupyter notebook (and run the necessary code snippets) while going through different sections of this chapter. Here is the GitHub link to the notebook corresponding to this chapter: https://github.com/PacktPublishing/Hands-On-Image-Processing-and-Computer-Vision-with-Python-Second-Edition. It's necessary to have the right versions of the libraries installed prior to running the code in order for the code to run seamlessly. If there are version mismatches, the code