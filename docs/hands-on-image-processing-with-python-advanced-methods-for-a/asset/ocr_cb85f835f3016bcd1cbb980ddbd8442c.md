# Displaying an image with matplotlib.pylab

The imshow() function from matplotlib.pylab is widely used to display images represented as NumPy ndarray objects. It supports optional arguments such as cmap for colormaps, interpolation for controlling pixel interpolation, and aspect for adjusting the image aspect ratio. The command axis('off') removes axis ticks and labels for cleaner visualization.

Interpolation methods determine how pixel values are estimated when image pixels do not align exactly with display pixels, which is especially important for small or resized images. Common interpolation methods include 'nearest', 'bilinear', 'bicubic', 'gaussian', and 'lanczos' (you will learn more about them in Chapters 2 and 3). The next code snippet demonstrates the effect of different interpolation methods by displaying the same image with multiple interpolation options using imshow(), plt.subplots(), and plt.tight_layout().

```
im = mpimg.imread("images/monalisa.jpg") # read image as numpy ndarray
methods = [None, 'none', 'nearest', 'bilinear', 'bicubic', 'spline16', \
    'spline36', 'hanning', 'hamming', 'hermite', 'kaiser', 'quadric',\
    'catrom', 'gaussian', 'bessel', 'mitchell', 'sinc', 'lanczos']
fig, axes = plt.subplots(nrows=3, ncols=6, figsize=(12, 7), \
    subplot_kw={'xticks': [], 'yticks': []})
fig.subplots_adjust(hspace=0.05, wspace=0.05)
for ax, interp_method in zip(axes.flat, methods):
    ax.imshow(im, interpolation=interp_method)
    ax.set_title(str(interp_method))
plt.tight_layout(); plt.show()
```

If you run the preceding code, you should obtain a figure like the following:

![](bbox=[0.22, 0.64, 0.72, 0.88])

Figure 1.5 – Displaying images with different interpolation methods