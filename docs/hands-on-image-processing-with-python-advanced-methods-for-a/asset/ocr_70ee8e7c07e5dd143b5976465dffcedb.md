# Pixelating an image

Pixelation is performed by first selecting the image region to be pixelated, then dividing it into fixed-size pixel blocks, replacing all pixels within each block by a representative value (typically the average color), and finally reconstructing the image so that fine details are replaced by coarse block-like structures. We can reduce an image's resolution and then scale it back up to create a pixelated effect; it is implemented with the pixelate() function in the next code snippet. It works as follows:

+   - Downsampling: The image is resized to a smaller version using bilinear interpolation (Image.BILINEAR), reducing details while preserving some smoothness.
+   - Upsampling: The small image is scaled back to the original size using nearest-neighbor interpolation (Image.NEAREST), which enlarges each pixel block, creating a blocky, pixelated look.

Refer to the next code snippet:

```
def pixelate(im, factor):
    # Resize down smoothly
    im_small = im.resize((im.width//factor, im.height//factor), \
                          resample=Image.BILINEAR)
    # Scale back up using NEAREST to original size
    return im_small.resize(im.size, Image.NEAREST)

im = Image.open("images/parrot.png")
plt.figure(figsize=(15,6))
plt.imshow(np.hstack([np.array(im)] + \
                    [np.array(pixelate(im, factor)) for factor in [16,32]]))
```

If you run the preceding code snippet, you should obtain a figure like the next one:

Pixelating by downsampling to factor of 16 and 32

![](bbox=[0.09, 0.63, 0.85, 0.79])

Figure 2.20 – Pixelation effect with PIL

# Anonymizing a face

Now, let's apply the pixelation to a part of the image, for example, the face region in the input Mona Lisa image, where we want to hide the details. The pixelation effect can be applied to the region of interest (ROI) using NumPy slicing.