An animated GIF can be created using ImageIO by first loading a sequence of images (e.g., PNG files from a folder via glob.glob() and imageio.imread()) into a list of frames, then initializing a GIF writer with imageio.get_writer() where the duration parameter controls per-frame display time, and finally iterating through the frame list to build the animation using writer.append_data(), producing an output file such as world.gif.

Now, refer to the following code snippet:

```
from glob import glob
# read frames for the animation
frames = [imageio.imread(f) for f in glob('images/world/*.png')]
# Save frames as an animated GIF
with imageio.get_writer('images/world.gif', duration=0.1) as writer:
    for frame in frames:
        writer.append_data(frame)
```

# Image file formats and conversion in Python

Images can be stored in multiple file formats, each with different compression strategies, color support, and use cases. Common formats include BMP (uncompressed raster, large size, supports 8/24/32-bit color depth with optional alpha channel), PNG (lossless compression, supports transparency, ideal for web graphics and sharp edges), JPEG (lossy compression, optimized for photographs with smooth gradients but no transparency), GIF (limited to 256 colors, supports lossless compression and animation), PPM/PNM (simple uncompressed formats with very large file sizes, rarely used), and TIFF (high-quality, flexible format supporting lossless compression, layers, and metadata, widely used in professional imaging).

The following table illustrates how bit depth affects pixel representation for a 2 x 2 image:

| Bit Depth | Pixel Values (Decimal/Binary) | Image |
|---|---|---|
| 8-bit | [[0, 85], [170, 255]]
Binary: [[00000000, 01010101], [10101010, 11111111]] | [2x2 grayscale image] |
| 16-bit (scaled to 8-bit for display) | [[0, 21845], [43690, 65535]]
Binary: [[0000000000000000, 0101010101010101], [1010101010101010, 1111111111111111]] | [2x2 grayscale image] |

Table 1.1: Bit depth and pixel representation