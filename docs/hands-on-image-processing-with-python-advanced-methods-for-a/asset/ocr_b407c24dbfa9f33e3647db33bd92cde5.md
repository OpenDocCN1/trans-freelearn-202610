If you run the preceding code snippet, you should obtain something similar to the following:

![](bbox=[0.22, 0.09, 0.7, 0.35])

Figure 3.31 – Annotating an image with text

# Drawing contour lines for an image

A contour line for an image is a curve connecting all the pixels where they have the same particular (constant) value. If we represent the image as a function, f(x, y), where x and y are the spatial coordinates, a contour line corresponds to a set of points (x, y) such that f(x, y) = constant.

The following code snippet shows how to draw the contour lines for a gray-scale image of Einstein, using the contour() function from the matplotlib.pylab module.

The constant levels are specified using the np.linspace() function, which creates an array with 15 values, evenly spaced between 0 and 255 (inclusive), to be used as levels (constants) for the contour plot.

Contour lines can be styled and colored using various parameters, such as colors, cmap (colormap; here plt.cm.hot is used), and linewidths. You can fill the regions between contours using plt.contourf().

You can add labels to the contours using the plt.clabel() function. This adds labels to the contours at specified locations (as can be seen from the following figures, the 18.2, 36.4, and 91.1 levels are added as labels. These values were generated previously using np.linspace()).

Now, refer to the following code snippet:

```
im = imread("images/einstein.jpg")  # read image as
                                    # a numpy ndarray

plt.subplot(131), plt.imshow(im, cmap='gray', aspect='auto')
plt.axis('off'), plt.title('Original Image', size=20)
plt.subplot(132)
cs = plt.contour(np.flipud(im), levels=np.linspace(0,255,15), \
                 cmap=plt.cm.hot)
plt.clabel(cs, fontsize=15, inline=True)
```