# Converting file formats

Despite format differences, Python image libraries abstract away metadata and allow direct access to image arrays and properties like size, mode, and data type. Format conversion can be performed using PIL by saving an image with a new extension (e.g., PNG to JPG), as demonstrated by the next code snippet:

```
im = Image.open("images/parrot.png")
print(im.mode)
# RGB
im.save("images/parrot.jpg")
```

PNG images in RGBA mode must first be converted to RGB before saving as JPEG, since JPEG does not support alpha channels, as shown in next code snippet.

```
im = Image.open("images/hill_alpha.png")
print(im.mode)
# RGBA
im.convert('RGB').save("images/hill.jpg") # first convert to RGB mode
```

# Image types (modes)

Images can be categorized based on number of channels used to represent each pixel.

+   - Single-channel images: Each pixel is represented by a single value:
+     - Binary (monochrome) images: Each pixel is stored using a single bit (0 or 1).
+     - Grayscale images: Each pixel typically uses 8 bits with intensity values in the range [0,255].
+   - Multi-channel images: Each pixel is represented by a tuple of values, for example:
+     - RGB / HSV images: Pixels are represented as (R, G, B) for red, green, and blue intensities, and as (H, S, V) for hue, saturation (colorfulness), and value (brightness), with HSV offering a more perceptually aligned color model.
+     - RGBA images: Four-channel images represented as (r, g, b, α), where α denotes transparency.
+     - Indexed color (palette) images: Pixels store indices into a color palette instead of direct RGB values. This reduces storage and is commonly used in GIF images.

# Converting image modes with PIL

PIL/Pillow supports multiple image modes, including:

+   - 1: 1-bit pixels, black and white, stored with one pixel per byte
+   - L: 8-bit pixels, black and white
+   - P: 8-bit pixels, mapped to any other mode using a palette
+   - RGB: 3 x 8-bit pixels, true color