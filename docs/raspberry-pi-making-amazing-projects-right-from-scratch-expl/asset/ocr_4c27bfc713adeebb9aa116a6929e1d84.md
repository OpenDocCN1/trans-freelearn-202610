# Creating Your Own Motion Detection and Tracking System

The way thresholding works is very simple. We define a threshold value. For a pixel in a grayscale image, if the value of grayscale intensity is greater than the threshold, then we assign a value to the pixel (for example, white); otherwise, we assign a black value to the pixel. This is the simplest form of thresholding. Also, there are many other variations of this method, which we will look at now.

In OpenCV, the `cv2.threshold()` function is used to threshold images. Its input includes grayscale image, threshold values, maxVal, and threshold methods as parameters and returns the thresholded image as the output. maxVal is the value assigned to the pixel if the pixel intensity is greater (or lesser in some methods) than the threshold. There are many threshold methods available in OpenCV; in the beginning, the simplest form of thresholding we saw was `cv2.THRESH_BINARY`. Let's look at the mathematical representation of some of the threshold methods.

Say (x,y) is the input pixel; then, operations for threshold methods will be as follows:

+   - `cv2.THRESH_BINARY`  
+     If intensity(x,y) > threshold, then set intensity(x,y)=maxVal; else, set intensity(x,y) = 0
+   
+   - `cv2.THRESH_BINARY_INV`  
+     If intensity(x,y) > threshold, then set intensity(x,y)=0; else, set intensity(x,y) = maxVal
+   
+   - `cv2.THRESH_TRUNC`  
+     If intensity(x,y) > threshold, then set intensity(x,y)=threshold; else, leave intensity(x,y) as it is
+   
+   - `cv2.THRESH_TOZERO`  
+     If intensity(x,y) > threshold, then leave intensity(x,y) as it is; else, set intensity(x,y) = 0
+   
+   - `cv2.THRESH_TOZERO_INV`  
+     If intensity(x,y) > threshold, then set intensity(x,y)=0; else, leave intensity(x,y) as it is

The demonstration of the threshold functionality usually works best on grayscale images with a gradually increasing gradient. In the following example, we are setting the value of the threshold as 127, so the image is segmented in two sets of pixels depending on the value of their intensity:

```
import cv2
import matplotlib.pyplot as plt
```