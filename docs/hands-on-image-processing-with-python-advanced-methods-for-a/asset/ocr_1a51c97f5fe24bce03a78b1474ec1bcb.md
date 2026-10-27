+   - **Residual skip connection:**
+   
+     $F_{i+1} = F_i + B(F_i)$
+   - **Long skip connection:**
+   
+     $F_{res} = F_0 + Conv(F_N)$
+   - **Upsampling layers:** Uses sub-pixel convolution or pixel shuffle [Shi et al., 2016]
+   - **Output layer:** Final convolution to get the RGB SR image

Now, let’s deep-dive into Python implementation and understand how the following code works. Here’s a concise summary of what the following code does, broken into key steps and functions:

+   - Loads and normalizes input image using Lambda(normalize) to center pixel values
+   - Defines residual blocks with res_block() using two convolutional layers and a skip connection
+   - Stacks multiple residual blocks (e.g., 16 blocks) and adds a long skip connection
+   - Applies upsampling via pixel_shuffle() in the upsample() function for 2×, 3×, or 4× resolution
+   - Builds the full model using edsr() that returns high-resolution output
+   - Loads pretrained weights from 'models/weights-edsr-16-x4.h5'
+   - Reads low-resolution input image using PIL.Image.open()
+   - Performs baseline upscaling via bicubic interpolation using PIL.Image.resize()
+   - Applies EDSR model inference using resolve_single() and resolve()
+   - Displays LR, bicubic, and EDSR outputs side by side using matplotlib.pyplot

Now refer to the following code snippet (to be run in Google Colab); for complete code, refer to GitHub:

```
# run in google colab (on GPU)
from tensorflow.keras.layers import Add, Conv2D, Input, Lambda
from tensorflow.keras.models import Model

def edsr(scale, num_filters=64, num_res_blocks=8, res_block_scaling=None):
    x_in = Input(shape=(None, None, 3))
    x = Lambda(normalize)(x_in)
    x = b = Conv2D(num_filters, 3, padding='same')(x)
    for i in range(num_res_blocks):
```