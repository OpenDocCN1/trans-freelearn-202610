# # Flipping

Image flipping is a geometric transformation that reverses pixel positions along an axis using `cv2.flip()`. The function accepts the input image and a `flipCode` parameter that specifies the flip direction: `flipCode=0` flips the image vertically (top ↔ bottom), `flipCode=1` flips it horizontally (left ↔ right), and `flipCode=-1` applies both flips simultaneously, equivalent to a 180° rotation. The example demonstrates all three flipping operations and compares them with the original image:

```
im = cv2.cvtColor(cv2.imread('images/parrot.png'),
                  cv2.COLOR_BGR2RGB)
plt.imshow(np.hstack((im,
                      cv2.flip(im, flipCode=0), # Flip Image Vertically
                      cv2.flip(im, flipCode=1), # Flip Image Horizontally
                      # Flip Image Vertically and Horizontally
                      cv2.flip(im, flipCode=-1))))
```

If you run the preceding code snippet, you should obtain something similar to the following figure:

![](bbox=[0.162, 0.398, 0.794, 0.496])

*Figure 3.17 – Flipping an image with opencv-python*

# ## When to use opencv-python over PIL

PIL (Pillow) is ideal for simple image manipulation tasks such as filtering, drawing, format conversion, and rapid prototyping due to its ease of use and readable API. OpenCV, implemented and optimized in C/C++, is generally preferred for computationally intensive image processing, real-time video applications, and computer vision tasks, offering significantly faster performance for operations such as resizing, warping, and filtering on high-resolution images or live video streams.

# ## Implementing a crosshatch filter

A crosshatch filter is an image-processing technique that enhances diagonal edges by applying a kernel that resembles crisscrossing lines, mimicking the traditional hatching technique used in drawing. The filter works by emphasizing diagonal features in an image through convolution with a structured kernel. The following code snippet implements a crosshatch filter using opencv-python. Let’s break the code down first:

+   - It loads an image using opencv-python's `imread()` function (note that it loads RGB images as BGR).
+   - The kernel is created, as follows:
+     - `np.eye(length, dtype=np.float32)`: Creates an identity matrix of size 24 × 24. The identity matrix has ones on the main diagonal and 0s elsewhere: