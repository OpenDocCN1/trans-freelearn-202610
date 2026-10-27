The Floyd–Steinberg dithering presented in the pseudocode outputs a binary (black-and-white) image, but this code is much more flexible and powerful! This version of the algorithm allows you to produce a dithered grayscale image with k distinct gray levels, not just 2! Whether you want to simulate vintage printers or compress image data while preserving visual richness, simply choose: `num_colors = k` (e.g., 2, 4, 8...) and the code will:

+   - Automatically create a palette of k equally spaced gray levels (e.g., [0, 85, 170, 255] for k=4)
+   - Quantize and diffuse pixel error intelligently to produce a natural-looking result
+   - Display and validate the number of unique tones in your output

Unlike the classical Floyd–Steinberg formulation, which is typically presented for binary quantization, this approach naturally extends to k-level grayscale quantization by selecting an appropriate number of output intensity levels (`num_colors=k`). The palette is then constructed as uniformly spaced gray levels, and the same error diffusion mechanism is applied without modification. This framework can be further generalized to color images by applying the dithering process independently to each channel.

Thresholding is fast; it is ideal for high-contrast applications like text. Halftoning suits traditional printing when grayscale isn’t available, while dithering excels at simulating smooth tones with minimal gray levels. The following table compares the above three methods:

| Feature | Thresholding | Halftoning | Dithering |
|---|---|---|---|
| Output | Binary | Patterned dots | Multi-tone illusion |
| Complexity | Very low | Medium | High |
| Spatial Context | None | Yes (patterned) | Yes (diffuses error) |
| Visual Quality | Low for photos | Medium–high | High |
| Customization | Static threshold | Grid patterns | Quantization + error |
| Hardware Use | Scanners | Printers | Displays, encoders |

*Table 7.4 – Thresholding vs. Halftoning vs. Dithering*

# Image enhancement with histogram processing

Histogram processing techniques provide a better method for altering the dynamic range of pixel values in an image so that its intensity histogram has a desired shape. As we have seen, image enhancement by the contrast stretching operation is limited in the sense that it can apply only linear scaling functions.