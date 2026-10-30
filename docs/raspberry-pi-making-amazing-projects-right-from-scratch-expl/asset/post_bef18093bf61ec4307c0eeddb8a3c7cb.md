
## Creating Your Own Motion Detection and Tracking System

In the previous chapter, we studied the basics of the OpenCV library. We set up our Pi for OpenCV programming with Python and implemented a simple project to track an object based on the color in OpenCV with a live webcam feed. In this chapter, we will learn about some more advanced concepts and implement one more project based on OpenCV. In this chapter, we will learn about the following topics:

- Thresholding
- Noise reduction
- Morphological operations on images
- Contours in OpenCV
- Real-time motion detection and tracking

## Thresholding images

**Thresholding** is a way to segment images. Although thresholding methods and algorithms are available for colored images, it works best on grayscale images. Thresholding usually (but not always) converts grayscale images into binary images (in a binary image, each pixel can have only one of the two possible values: white or black). Thresholding the image is usually the first step in many image processing applications.

The way thresholding works is very simple. We define a threshold value. For a pixel in a grayscale image, if the value of grayscale intensity is greater than the threshold, then we assign a value to the pixel (for example, white); otherwise, we assign a black value to the pixel. This is the simplest form of thresholding. Also, there are many other variations of this method, which we will look at now.

In OpenCV, the `cv2.threshold()` function is used to threshold images. Its input includes grayscale image, threshold values, maxVal, and threshold methods as parameters and returns the thresholded image as the output. maxVal is the value assigned to the pixel if the pixel intensity is greater (or lesser in some methods) than the threshold. There are many threshold methods available in OpenCV; in the beginning, the simplest form of thresholding we saw was `cv2.THRESH_BINARY`. Let's look at the mathematical representation of some of the threshold methods.

Say (x,y) is the input pixel; then, operations for threshold methods will be as follows:

- `cv2.THRESH_BINARY`  
  If intensity(x,y) > threshold, then set intensity(x,y)=maxVal; else, set intensity(x,y) = 0
- `cv2.THRESH_BINARY_INV`  
  If intensity(x,y) > threshold, then set intensity(x,y)=0; else, set intensity(x,y) = maxVal
- `cv2.THRESH_TRUNC`  
  If intensity(x,y) > threshold, then set intensity(x,y)=threshold; else, leave intensity(x,y) as it is
- `cv2.THRESH_TOZERO`  
  If intensity(x,y) > threshold, then leave intensity(x,y) as it is; else, set intensity(x,y) = 0
- `cv2.THRESH_TOZERO_INV`  
  If intensity(x,y) > threshold, then set intensity(x,y)=0; else, leave intensity(x,y) as it is

The demonstration of the threshold functionality usually works best on grayscale images with a gradually increasing gradient. In the following example, we are setting the value of the threshold as 127, so the image is segmented in two sets of pixels depending on the value of their intensity:

```python
import cv2
import matplotlib.pyplot as plt

img = cv2.imread('gray21.512.tiff',0)
th=127
max_val=255
ret,o1 = cv2.threshold(img,th,max_val,cv2.THRESH_BINARY)
ret,o2 = cv2.threshold(img,th,max_val,cv2.THRESH_BINARY_INV)
ret,o3 = cv2.threshold(img,th,max_val,cv2.THRESH_TOZERO)
ret,o4 = cv2.threshold(img,th,max_val,cv2.THRESH_TOZERO_INV)
ret,o5 = cv2.threshold(img,th,max_val,cv2.THRESH_TRUNC)

titles = ['Input Image','BINARY','BINARY_INV','TOZERO',
          'TOZERO_INV','TRUNC']
output = [img, o1, o2, o3, o4, o5]

for i in xrange(6):
    plt.subplot(2,3,i+1),plt.imshow(output[i],cmap='gray')
    plt.title(titles[i])
    plt.xticks([]),plt.yticks([])
plt.show()
```

The output of the preceding code will be as follows:

![](img/631e8b200cce187082ee46b0eb1681b3_151_0.png)

### Otsu's method

Otsu's method for thresholding automatically determines the value of the threshold for images that have two peaks in their histogram (bimodal histograms). The following is a bimodal histogram:

![](img/631e8b200cce187082ee46b0eb1681b3_152_0.png)

This usually means that the image has background and foreground pixels and Otsu's method is the best way to separate these two sets of pixels automatically without specifying a threshold value.

Otsu's method is not the best way for those images that are not in the background and foreground model and may provide improper output if applied.

This method is applied in addition to other methods, and the threshold is passed as 0. Try out the following code:

```python
ret,output=cv2.threshold(image,0,255,cv2.THRESH_BINARY+cv2.THRESH_OTSU)
```

The output of this will be as follows. This is a screenshot of a tank in a desert:

![](img/631e8b200cce187082ee46b0eb1681b3_153_0.png)

## Noise

**Noise** means an unwanted signal. Image/video noise means unwanted variations in intensity (for grayscale image) or colors (for color images) that are not present in the real object being photographed or recorded. Image noise is a form of electronic disruption and could come from many sources, such as camera sensors and circuitry in digital or analog cameras. Noise in digital cameras is equivalent to the film grain of analog cameras. Though some noise is always present in any output of electronic devices, a high amount of image noise considerably degrades the overall image quality, making it useless for the intended purpose. To represent the quality of the electronic output (in our case, digital images), the mathematical term **signal-to-noise ratio (SNR)** is a very useful term. Mathematically, it's defined as follows:

$$SNR = \frac{SignalPower}{NoisePower}$$

> More signal-to-noise ratio translates into better quality image.

### Kernels for noise removal

In the following concepts and their implementations, we are going to use kernels. **Kernels** are square matrices used in image processing. We can apply a kernel to an image to get different results, such as the blurring, smoothing, edge detection, and sharpening of an image. One of the main uses of kernels is to apply a low pass filter to an image. Low pass filters average out rapid changes in the intensity of the image pixels. This basically smoothens or blurs the image. A simple averaging kernel can be mathematically represented as follows:

$$K = \frac{All\ Ones\ Matrix}{Rows * Cols}$$

For row = cols = 3, kernel will be as follows:

$$K = \frac{\begin{bmatrix}1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1\end{bmatrix}}{9}$$

The value of the rows and columns in the kernel is always odd.

We can use the following NumPy code to create this kernel:

```python
K=np.ones((3,3),np.uint32)/9
```

### 2D convolution filtering

cv2.filter2D() function convolves the previously mentioned kernel with the image, thus applying a linear filter to the image. This function accepts the source image and depth of the destination image (-1 in our case, where -1 means the same depth as the source image) and a kernel. Take a look at the following code. It applies a 7 x 7 averaging filter to an image:

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

img = cv2.imread('4.2.03.tiff',1)

input = cv2.cvtColor(img,cv2.COLOR_BGR2RGB)
output = cv2.filter2D(input,-1,np.ones((7,7),np.float32)/49)

plt.subplot(121),plt.imshow(input),plt.title('Input')
plt.xticks([]), plt.yticks([])
plt.subplot(122),plt.imshow(output),plt.title('Output')
plt.xticks([]), plt.yticks([])
plt.show()
```

The output will be a filtered image, as follows:

![](img/631e8b200cce187082ee46b0eb1681b3_155_0.png)

### Low pass filtering

As discussed in the kernels section, low pass filters are excellent when it comes to removing sharp components (high frequency information), such as edges and noise, and retaining low frequency information (so-called low pass filters), thus blurring or smoothening them.

Let's explore the low pass filtering functions available in OpenCV. We do not have to create and pass the kernel as an argument to these functions; instead, these functions create the kernel based on the size of the kernel we pass as the parameter.

cv2.boxFilter() function takes the image, depth, and size of the kernel as inputs and blurs the image. We can specify normalize as either True or False. If it's True, the matrix in the kernel would have 1/(rows*cols) as its coefficient; thus, the matrix is called a normalized box filter. If normalize is False, then the coefficient will be 1, and it will be an unnormalized box filter. An unnormalized box filter is useful for the computing of various integral characteristics over each pixel neighborhood, such as covariance matrices of image derivatives (used in dense optical flow algorithms, and so on). The following code demonstrates a normalized box filter:

```python
output=cv2.boxFilter(input,-1,(3,3),normalize=True)
```

The output of the code will be as follows, and it will have less smoothing than the previous one due to the size of the kernel matrix:

![](img/631e8b200cce187082ee46b0eb1681b3_156_0.png)

The cv2.blur() function directly provides the normalized box filter by accepting the input image and the kernel size as parameters without the need to specify the normalize parameter. The output for the following code will be exactly the same as the preceding output:

```python
output = cv2.blur(input,(3,3))
```
