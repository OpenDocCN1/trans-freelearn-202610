In this example, we are reading an image in grayscale and displaying it using `matplotlib`. The following screenshot shows the plot of the image:

![](bbox=[0.176, 0.19, 0.824, 0.612])

The `plt.xticks([])` and `plt.yticks([])` functions can be used to disable x and y axis. Run the preceding code again, and this time, comment out the two lines with the `plt.xticks([])` and `plt.yticks([])` functions.

The `cv2.imread()` OpenCV function reads images and saves them as NumPy arrays of Blue, Green, and Red (BGR) pixels.