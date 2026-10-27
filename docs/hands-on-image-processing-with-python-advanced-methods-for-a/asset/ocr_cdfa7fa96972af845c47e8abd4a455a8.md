```
.convert('RGBA')
layer = Image.new('RGBA', cover_image.size, (0, 0, 0, 0))
layer.paste(watermark, (325, 225))

# Create a copy of the layer
layer2 = layer.copy()
# Put alpha on the copy
layer2.putalpha(128)
# merge layers with mask
layer.paste(layer2, layer)

watermarked_image = Image.alpha_composite(cover_image, layer)
watermarked_image.convert('RGB')
```

If you run the preceding code snippet, you should obtain a watermarked image like the next one:

![](bbox=[0.25, 0.39, 0.65, 0.66])

Figure 2.42 – Adding a watermark (logo) to an image

# Blending two images with a mask

The next code snippet blends a butterfly image (im) with its own inverted version (im_inv) using a (horizontal) intensity gradient transparency mask (grad_mask), with the Image.composite() method. Here are the key steps.

First, a 1D gradient array (grad_mask) is created with np.linspace(), and the np.tile(grad_mask, (h, 1)) function is used to repeat the 1D gradient array across rows to create a 2D horizontal intensity gradient mask, where the pixel values range from 0 (fully transparent) to 255 (fully opaque).

The Image.composite(im, im_inv, grad_mask) method is used to blend im and im_inv based on the transparency mask, grad_mask, making one side more like the original and the other more like the inverted version.