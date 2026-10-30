# Colorspaces and conversions

A **colorspace** is a mathematical model used to represent colors. Usually, colorspaces are used to represent colors in a numerical form and perform mathematical and logical operations with them. In this book, the colorspaces we mostly use are BGR (OpenCV's default colorspace), RGB, HSV, and grayscale. BGR stand for Blue, Green, and Red. HSV represents colors in the Hue, Saturation, and Value format. OpenCV has a `cv2.cvtColor(img, conv_flag)` function that allows us to change the colorspace of an img image, while the source and target colorspaces are indicated in the `conv_flag` parameter. We have learned that OpenCV loads images in the BGR format, and matplotlib uses the RGB format for images. So, before displaying images with matplotlib, we need to convert images from BGR to the RGB colorspace. Take a look at the following code. The programs read image in the color mode using `cv2.imread()`, which imports the image in the BGR colorspace. Then, it converts it into RGB using `cv2.cvtColor()`, and finally, it uses matplotlib to display the image:

```
import cv2
import matplotlib.pyplot as plt

img = cv2.imread('4.2.07.tiff',1)
img = cv2.cvtColor( img , cv2.COLOR_BGR2RGB )
plt.imshow( img ), plt.title('COLOR IMAGE'), plt.xticks([]),
    plt.yticks([])
plt.show()
```

Another way to convert an image from BGR to RGB is to first split the image into three separate channels (B, G, and R channels) and merge them in the BGR order. However, this takes more time as split and merge operations are inherently computationally costly, making them slower and inefficient. The following code shows this method:

```
import cv2
import matplotlib.pyplot as plt
img = cv2.imread('4.2.07.tiff',1)
b,g,r = cv2.split( img )
img=cv2.merge((r,g,b))
plt.imshow( img ), plt.title('COLOR IMAGE'), plt.xticks([]),
    plt.yticks([])
plt.show()
```