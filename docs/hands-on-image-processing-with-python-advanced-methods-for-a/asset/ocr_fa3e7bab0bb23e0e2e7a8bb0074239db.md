# With opencv-python

The cv2.getPerspectiveTransform() function can be used to compute the perspective transformation matrix. This matrix can be used to perform a perspective transformation on an image. Here is how it can be used to implement homography:

+   - First, you need to have four pairs of corresponding points in the source and destination coordinate spaces. Each pair should be a tuple of (x, y) coordinates, as shown in the following code snippet:

```
im_src = (imread('images/books.jpg'))
height, width, dim = im_src.shape
src = np.array([[267., 364.],
                [312., 683.],
                [555., 598.],
                [561., 284.]], dtype='float32')
dst = np.array([[  0.,   0.],
                [0, height-1],
                [width-1, height-1],
                [width-1, 0.]], dtype='float32')

def plot_images(src, dst):
    plt.figure(figsize=(7,7))
    plt.subplot(121), plt.imshow(src), plt.axis('off')
    plt.title('Source image', size=15)
    plt.subplot(122), plt.imshow(dst), plt.axis('off')
    plt.title('Destination image', size=15)
    plt.tight_layout()
    plt.show()
```

+   - Use cv2.getPerspectiveTransform() to compute the perspective transformation matrix based on the corresponding points. The numpy ndarray perspective_matrix represents a 3x3 matrix corresponding to the perspective transformation.

+   - Once you have the perspective transformation matrix, you can use it to transform coordinates or warp an image, using the cv2.warpPerspective() function:

```
%timeit M = cv2.getPerspectiveTransform(src, dst)
%timeit im_dst = cv2.warpPerspective(im_src, M, (width, height), flags=cv2.INTER_LINEAR)
# plot_images(im_src, im_dst)
# 4.22 µs±160 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
# 7.21 ms±385 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
```