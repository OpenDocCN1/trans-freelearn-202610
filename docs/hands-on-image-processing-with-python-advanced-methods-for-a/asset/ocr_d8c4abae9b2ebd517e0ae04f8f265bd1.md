Then the output image q is obtained by averaging all q_i values using a box filter.

Let's now explore an edge enhancement application using guided filtering with opencv-python using the following code snippet:

```
from cv2.ximgproc import guidedFilter
# Load grayscale image and resize for faster processing
img = cv2.imread('images/we.jpg', cv2.IMREAD_GRAYSCALE)
img = cv2.resize(img, (256, 256))
# Normalize to float32 [0, 1]
img_float = img.astype(np.float32) / 255.0
# Step 1: Compute gradient magnitude
grad_x = cv2.Sobel(img_float, cv2.CV_32F, 1, 0, ksize=3)
grad_y = cv2.Sobel(img_float, cv2.CV_32F, 0, 1, ksize=3)
grad_mag = np.sqrt(grad_x**2 + grad_y**2)
# Step 2: Apply guided filter (guide = original image, input = grad_mag)
radius = 8
eps = 1e-3  # Regularization term
grad_mag_smooth = guidedFilter(guide=img_float, src=grad_mag, \
                                radius=radius, eps=eps)
# Step 3: Edge enhancement by subtraction
enhanced_edges = grad_mag - grad_mag_smooth
enhanced_edges = np.clip(enhanced_edges, 0, 1)
# Display img, grad_mag and enhanced_edges side-by-side here
```

If you run the above code and plot the images, you should obtain a figure like the following:

![](bbox=[0.18, 0.6, 0.86, 0.75])

Figure 8.24 – Edge enhancement with guided filtering with opencv-python ximgproc

The function guidedFilter() accepts the following parameters:

+   - guide: the original image
+   - src: the gradient image to be smoothed
+   - radius: window size ω
+   - eps: regularization parameter ε