+   - RGBA: 4 x 8-bit pixels, true color with transparency
+   - CMYK: 4 x 8-bit pixels, color separation
+   - F: 32-bit floating-point pixels

# From RGB to binary mode

We can use the convert() method from PIL.Image to convert an RGB image binary image (mode = '1') using the following line of code:

```
Image.open('images/parrot.png').convert('1', dither=Image.FLOYDSTEINBERG)
```

If you run the preceding line of code, you should obtain an an image like next one:

![](bbox=[0.31, 0.27, 0.64, 0.48])

Figure 1.14 – Binary image with Floyd-Steinberg dithering

# Note

The preceding binary image uses only two different pixel values (namely, black and white, and if converted to a NumPy ndarray, it produces an array of bool), but it still looks like the original image without much loss of information since it uses the dithering algorithm (FLOYD-STEINBERG, in this case), which we will discuss in detail later in Chapter 6 (the Image Enhancement chapter):

```
np.unique(np.array(Image.open('images/parrot.png').convert('1')))
# array([False, True], dtype=bool)
```

# From RGB to palette mode and back

In palette ("P") mode, pixels store indices that map to RGB colors in a lookup table. Each pixel's value is not a color directly but a reference (e.g., an integer index from 0–255) to a separate palette list, which contains the actual RGB values. This mapping helps compress the image size when the total number of colors is small, which is why formats such as GIF use this technique. The following table visually illustrates how palette mode works for a 2 x 2 (RGB) color image, and how each pixel in the image holds an index value that maps to an actual color in a palette: