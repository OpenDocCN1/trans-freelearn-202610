# # Creating Your Own Motion Detection and Tracking System

In the previous chapter, we studied the basics of the OpenCV library. We set up our Pi for OpenCV programming with Python and implemented a simple project to track an object based on the color in OpenCV with a live webcam feed. In this chapter, we will learn about some more advanced concepts and implement one more project based on OpenCV. In this chapter, we will learn about the following topics:

+   - Thresholding
+   - Noise reduction
+   - Morphological operations on images
+   - Contours in OpenCV
+   - Real-time motion detection and tracking

# ## Thresholding images

**Thresholding** is a way to segment images. Although thresholding methods and algorithms are available for colored images, it works best on grayscale images. Thresholding usually (but not always) converts grayscale images into binary images (in a binary image, each pixel can have only one of the two possible values: white or black). Thresholding the image is usually the first step in many image processing applications.