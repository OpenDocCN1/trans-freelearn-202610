The next Python code pixelates a specified rectangular region of the Mona Lisa image by replacing it with the corresponding area from a pixelated version (obtained with the pixelate() function), preserving the rest of the image. It can be useful for **privacy protection** (e.g., blurring faces or sensitive information), **anonymization** in media, **focus effects** in artistic edits, and **data preprocessing** for computer vision tasks such as facial recognition obfuscation:

```
im = Image.open("images/monalisa.jpg")
im_p = pixelate(im, 12)
x, y, h, w = 120, 120, 90, 115 #105, 105, 55, 45  # specify rect region
out = np.array(im)
out[x:x+h, y:y+w] = np.array(im_p)[y:y+h, x:x+w]
plt.imshow(np.hstack((np.array(im), out))), plt.axis('off')
```

If you run the preceding code snippet, you should obtain a figure like the next one.

![](bbox=[0.21, 0.34, 0.82, 0.68])

Figure 2.21 – Anonymizing face by pixelating

# Applying point transformations

As described earlier, point transformation in image processing refers to operations that modify the intensity (or color) of each pixel individually, without considering the pixel's neighborhood. The transformation is defined by a mathematical function applied to each pixel value of the input image to produce a new value in the output image. The general form of a point transformation can be written as follows:

$I_{output}(x,y) = T\left(I_{input}(x,y)\right)$ or, in sort, $s = T(r)$