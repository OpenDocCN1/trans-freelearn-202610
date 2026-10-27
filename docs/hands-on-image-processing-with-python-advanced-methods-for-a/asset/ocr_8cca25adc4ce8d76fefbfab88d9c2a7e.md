As we have seen earlier, filters modify pixel values using mathematical transformations such as color mapping, gamma correction, and convolution. Filters typically involve the following:

+   - **Color transformations:** Adjust RGB values using a transformation matrix:
+   
+   $$egin{bmatrix}R'\G'\B'\end{bmatrix}=M\times\begin{bmatrix}R\G\B\end{bmatrix}+B$$
+   
+   Here, $M$ is a transformation matrix and $B$ is a bias vector.

+   - **Gamma correction:** Adjust the brightness using $I' = I^\gamma$, where $\gamma$ controls brightness.

+   - **Convolution filters:** Apply a kernel, $K$, to enhance features:
+   
+   $$I'(x,y)=\sum_{i=-k}^{k}\sum_{j=-k}^{k}I(x-i,y-j)K(i,j)$$
+   
+   Here, $K$ is a filter kernel (e.g., Gaussian blur or sharpening).

The following Python code snippet applies multiple Instagram-like filters from the Pilgram library to an image and displays them in a grid. Here are the key steps:

+   1. The getmembers(pilgram, isfunction) function retrieves all filter functions available in the Pilgram library.
+   2. The Mona Lisa image is loaded from disk, then each filter function is applied to this (original) image, im, and the resulting images are stored in a dictionary named effects, with the filter name as the key.
+   3. The original image, along with the filtered images (and the names of the applied filters as titles) are displayed in a grid using Matplotlib, by iterating over the effects dictionary.

Now, refer to the following code snippet:

```
# !pip install pilgram
import pilgram
# dir(pilgram)
from PIL import Image
from inspect import getmembers, isfunction
import matplotlib.pyplot as plt

im = Image.open('images/monalisa.jpg')
effects = {'original': im}
for f in getmembers(pilgram, isfunction):
```