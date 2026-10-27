# Color spaces

A color space is a mathematical representation of colors using coordinate systems that enable efficient image representation, manipulation, and analysis. Different color spaces are designed for different image-processing tasks, since no single representation is optimal for all applications.

Commonly used color spaces include the following:

+   - **RGB (Red, Green, Blue)**: The standard additive color model used in electronic displays and general image processing.
+   - **HSV (Hue, Saturation, Value)**: Separates color, colorfulness, and brightness, making it useful for color filtering, segmentation, and color manipulation.
+   - **CMYK (Cyan, Magenta, Yellow, Black)**: A subtractive color model primarily used in printing and publishing.
+   - **Lab (CIELAB)**: Represents colors using lightness (L*) and opponent color channels (a, b). It is perceptually uniform and widely used for color correction and comparison.
+   - **YUV**: Separates luminance and chrominance components, enabling efficient video compression and broadcasting.
+   - **YCbCr**: Separates luma (Y) from chroma (Cb, Cr) and is extensively used in JPEG, MPEG, and digital video compression.
+   - **YIQ**: A television broadcasting color model used in the NTSC standard, separating luminance and chromaticity information.

Different color spaces emphasize different image characteristics such as brightness, chromaticity, perceptual uniformity, or compression efficiency. For example, RGB is intuitive and hardware-friendly, HSV simplifies color-based thresholding, Lab aligns closely with human color perception, YUV/YCbCr improve compression efficiency, and Grayscale reduces computational complexity for tasks such as OCR and edge detection.

Choosing an appropriate color space depends on the application requirements, computational needs, and perceptual properties relevant to the image-processing task.

We can use affine mappings to go from one color space to another. The following matrices represent the linear mapping from RGB to the YIQ color space:

RGB to YIQ
[ Y ]   [ 0.299   0.587   0.114 ] [ R ]
[ I ] = [ 0.596  -0.274  -0.322 ] [ G ]
[ Q ]   [ 0.211  -0.523   0.312 ] [ B ]

YIQ to RGB
[ R ]   [ 1   0.956   0.621 ] [ Y ]
[ G ] = [ 1  -0.272  -0.647 ] [ I ]
[ B ]   [ 1  -1.106   1.703 ] [ Q ]

Figure 1.18 – Colorspace transformation matrices

# Color gamut

Color gamut refers to the range of colors that a device or system can reproduce or display, typically represented as a subset of the visible spectrum in a color space defined by primary colors. A wider