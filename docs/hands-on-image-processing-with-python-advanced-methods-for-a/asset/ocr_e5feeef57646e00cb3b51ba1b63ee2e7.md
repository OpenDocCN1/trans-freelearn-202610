# Plotting coordinates (Matplotlib-style)

Matplotlib’s general plotting functions (e.g., plt.plot() and plt.scatter()) uses standard Cartesian coordinates: the origin, (0, 0), is at the bottom left by default, X increases to the right, and Y increases upward. However, when displaying images with plt.imshow(), the image is still displayed bottom-up (origin at bottom left) unless overridden. The next table summarizes the differences in the coordinate systems:

| Feature | NumPy Array | Matplotlib Plot |
|---|---|---|
| Indexing format | (row, col) = (y, x) | (x, y) |
| Origin (default) | Top left | Bottom left |
| Y axis direction | Top to bottom | Bottom to top (unless origin='upper') |
| Use | Processing and filtering | Display and overlays |

Table 1.4: Coordinate system for numpy 2D array vs. matplotlib plot

# Basic image manipulation

Different Python libraries can be used for basic image manipulation. Almost all the libraries store an image in a NumPy ndarray (2D array for grayscale, 3D array for RGB image, for example). As explained earlier, the following image shows the positive x and y directions (origin being the top-left corner of the image 2D array) for the colored Mona Lisa image:

![](bbox=[0.36, 0.51, 0.67, 0.73])

Figure 1.33 – Image versus NumPy 2D array coordinates

# Common image manipulation operations

Digital image manipulation involves modifying images to enhance appearance, change composition, or create visual effects. Common operations include cropping, resizing, rotating, color adjustment, sharpening, blurring, warping, and applying filter effects such as sepia or grayscale. More advanced manipulations include adding text, removing objects, cloning image regions, and merging multiple images into composite scenes. Many of these techniques will be implemented throughout this book using different Python image-processing libraries.