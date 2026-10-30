

In this example, we are reading an image in grayscale and displaying it using `matplotlib`. The following screenshot shows the plot of the image:

![](img/631e8b200cce187082ee46b0eb1681b3_131_0.png)

The `plt.xticks([])` and `plt.yticks([])` functions can be used to disable x and y axis. Run the preceding code again, and this time, comment out the two lines with the `plt.xticks([])` and `plt.yticks([])` functions.

The `cv2.imread()` OpenCV function reads images and saves them as NumPy arrays of Blue, Green, and Red (BGR) pixels.

## API 文档

### 1. 简介

本 API 用于管理用户账户、商品信息和订单数据。所有接口均返回 JSON 格式的数据。

### 2. 身份认证

除登录接口外，所有接口都需要在请求头中携带 Token：

```
Authorization: Bearer <token>
```

### 3. 用户接口

- `GET /users` - 获取用户列表
- `POST /users` - 创建用户
- `GET /users/{id}` - 获取用户信息
- `PUT /users/{id}` - 更新用户信息
- `DELETE /users/{id}` - 删除用户

### 4. 商品接口

- `GET /products` - 获取商品列表
- `POST /products` - 创建商品
- `GET /products/{id}` - 获取商品详情
- `PUT /products/{id}` - 更新商品信息
- `DELETE /products/{id}` - 删除商品

You can find out the number of cameras and associated device indexes using the following command:

```
ls -l /dev/video*
```

Once `cam.read()` returns a Boolean value `ret` and the frame, which is the image it captured. If the image capture is successful, then `ret` will be True; otherwise, it will be False. The previously listed code will capture an image with the camera device, `/dev/video0`, display it, and then save it. `cam.release()` will release the device.

This code can be used with slight modifications to display live video stream from the webcam:

```
import cv2

cam = cv2.VideoCapture(0)
print 'Default Resolution is ' + str(int(cam.get(3))) + 'x' + str(int(cam.get(4)))
w=1024
h=768
cam.set(3,w)
cam.set(4,h)
print 'Now resolution is set to ' + str(w) + 'x' + str(h)

while(True):
    # Capture frame-by-frame
    ret, frame = cam.read()

    # Display the resulting frame
    cv2.imshow('Video Test',frame)

    # Wait for Escape Key
    if cv2.waitKey(1) == 27 :
        break

# When everything done, release the capture
cam.release()
cv2.destroyAllWindows()
```

You can access the features of the video device with `cam.get(propertyID)`. 3 stands for the width and 4 stands for the height. These properties can be set with `cam.set(propertyID, value)`.

The preceding code first displays the default resolution and then sets it to 1024 x 768 and displays the live video stream till the Esc key is pressed. This is the basic skeleton logic for all the live video processing with OpenCV. We will make use of this in future.

## Saving a video using OpenCV

We need to use the `cv2.VideoWriter()` function to write a video to a file. Take a look at the following code:

```
import cv2
cam = cv2.VideoCapture(0)
output = cv2.VideoWriter('VideoStream.avi',
cv2.cv.CV_FOURCC(*'WMV2'),40.0,(640,480))

while (cam.isOpened()):
    ret, frame = cam.read()
    if ret == True:
        output.write(frame)
        cv2.imshow('VideoStream', frame )
        if cv2.waitKey(1) == 27 :
            break
    else:
        break

cam.release()
output.release()
cv2.destroyAllWindows()
```

In the preceding code, `cv2.VideoWriter()` accepts the following parameters:

- **Filename:** This is the name of the video file.
- **FourCC:** This stands for Four Character Code. We have to use the `cv2.cv.CV_FOURCC()` function for this. This function accepts FourCC in the `*'code'` format. This means that for DIVX, we need to pass `*'DIVX'`, and so on. Some supported formats are DIVX, XVID, H264, MJPG, WMV1, and WMV2.
- **Framerate:** This is the rate of the frames to be captured per second.
- **Resolution:** This is the resolution of the video to be captured.

> You can read more about FourCC at www.fourcc.org.

The preceding code records the video till the *Esc* key is pressed and saves it in the specified file.

## Pi Camera and OpenCV

The following code demonstrates the use of Picamera with OpenCV. It shows a preview for 3 seconds, captures an image, and displays it on screen using `cv2.imshow()`:

```python
import picamera
import picamera.array
import time
import cv2

with picamera.PiCamera() as camera:
    rawCap=picamera.array.PiRGBArray(camera)
    camera.start_preview()
    time.sleep(3)
    camera.capture(rawCap,format="bgr")
    image=rawCap.array
    cv2.imshow("Test",image)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
```

## Retrieving image properties

We can retrieve and use many image properties with OpenCV functions. Take a look at the following code:

```python
import cv2
img = cv2.imread('lena_color_512.tif',1)
print img.shape
print img.size
print img.dtype
```

The `img.shape` operation returns the shape of the image, that is, its dimensions and the number of color channels. The output of the previously listed code will be as follows:

```text
(512, 512, 3)
786432
uint8
```

If the image is colored, then `img.shape` returns a triplet containing the number of rows, the number of columns, and the number of channels in the image. Usually, the number of channels is three, representing the red, green, and blue channels. If the image is grayscale, then `img.shape` only returns the number of rows and the number of columns. Try to modify the preceding code to read the image in the grayscale mode and observe the output of `img.shape`.

The `img.size` operation returns the total number of pixels, and `img.dtype` returns the image datatype.

## Arithmetic operations on images

In this section, we will take a look at the various arithmetic operations that can be performed on images. Images are represented as matrices in OpenCV. So, arithmetic operations on images are the same as arithmetic operations on matrices. Images must be of the same size in order to perform arithmetic operations with images, and these operations are performed on individual pixels. `cv2.add()` method is used to add two images, where images are passed as parameters.

The `cv2.subtract()` method is used to subtract one image from another.

> We know that subtraction operation is not commutative; so, `cv2.subtract(img1,img2)` and `cv2.subtract(img2,img1)` will yield different results, whereas `cv2.add(img1,img2)` and `cv2.add(img2,img1)` will yield the same result as the addition operation is commutative. Both the images have to be of the same size and type as that explained earlier.

Check out the following code:

```
import cv2
img1 = cv2.imread('4.2.03.tiff',1)
img2 = cv2.imread('4.2.04.tiff',1)
cv2.imshow('Image1',img1)
cv2.waitKey(0)
cv2.imshow('Image2',img2)
cv2.waitKey(0)
cv2.imshow('Addition',cv2.add(img1,img2))
cv2.waitKey(0)
cv2.imshow('Image1-Image2',cv2.subtract(img1,img2))
cv2.waitKey(0)
cv2.imshow('Image2-Image1',cv2.subtract(img2,img1))
cv2.waitKey(0)
cv2.destroyAllWindows()
```

The preceding code demonstrates the usage of arithmetic functions on images. Image2 is the same *Lena* image that we experimented with in the previous chapter, so I am not including its output window. The following is the output window of **Image1**:

![](img/631e8b200cce187082ee46b0eb1681b3_137_0.png)

The following is the output of the **Addition**:

![](img/631e8b200cce187082ee46b0eb1681b3_138_0.png)

The following is the output window of **Image1-Image2**:

![](img/631e8b200cce187082ee46b0eb1681b3_139_0.png)

The following is the output window of **Image2-Image1**:

![](img/631e8b200cce187082ee46b0eb1681b3_140_0.png)

## Splitting and merging image color channels

On several occasions, we might be interested in working separately with the red, green, and blue channels. For example, we might want to build a histogram for each channel of an image.

The `cv2.split()` method is used to split an image into three different intensity arrays for each color channel, whereas `cv2.merge()` is used to merge different arrays into a single multichannel array, that is, a color image. Let's take a look at an example:

```python
import cv2
img = cv2.imread('4.2.03.tiff',1)
b,g,r = cv2.split(img)
cv2.imshow('Blue Channel',b)
cv2.imshow('Green Channel',g)
cv2.imshow('Red Channel',r)
img=cv2.merge((b,g,r))
cv2.imshow('Merged Output',img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

The preceding program first splits the image into three channels (blue, green, and red) and then displays each one of them. The separate channels will only hold the intensity values of that color, and they will be essentially displayed as grayscale intensity images. Then, the program will merge all the channels back into an image and display it.

## Negating an image

In mathematical terms, the negative of an image is the inversion of colors. For a grayscale image, it is even simpler! The negative of a grayscale image is just the intensity inversion, which can be achieved by finding the complement of the intensity from 255. A pixel value ranges from 0 to 255 and, therefore, negation is the subtraction of the pixel value from the maximum value, that is, 255. The code for this is as follows:

```python
import cv2
img = cv2.imread('4.2.07.tiff')
grayscale = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
negative = abs(255-grayscale)
cv2.imshow('Original',img)
cv2.imshow('Grayscale',grayscale)
cv2.imshow('Negative',negative)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

> The negative of a negative will be the original grayscale image. Try this on your own by taking the image negative of a negative again.

![](img/631e8b200cce187082ee46b0eb1681b3_141_0.png)

