
This exploits the observation that in non-sky regions, at least one channel tends to have low intensity. In hazy images, these dark channels appear much brighter due to scattered light. Hence, the difference between the dark channel of a hazy image and a haze-free image becomes a cue for transmission estimation.

Let’s understand the algorithm step by step:

- Reads the input image.
- **Computes dark channel:** implemented by the function `dark_channel()`, which splits RGB channels using a minimum filter and morphological erosion on local patches, computes the minimum per-pixel across R, G, B and applies erosion with a kernel (patch size sz) to compute the local minimum — the dark channel.
- **Estimates global atmospheric light:** Implemented using the function `estimate_atmospheric_light()`, refer to the next code snippet. We assume the brightest pixels in the dark channel correspond to haze-opaque regions (e.g., sky or distant background). Their RGB values give us the atmospheric light vector A.
- **Flattens the dark channel and selects the top brightest pixels:** Averages their RGB values to estimate atmospheric light A.
- **Estimates transmission map** (dehazes the image) using the function `dehaze()`. We use the equation derived from the haze model and assume constant airlight over a local patch:
  
  $$t(x) = 1 - \omega \cdot I_{dark}(x)$$ 
  
  where $\omega$ is a constant (typically 0.95).
- **Recover scene radiance from the haze model:**
  
  $$J(x) = \frac{I(x) - A}{\max(t(x), t0)} + A$$
- Displays original and restored images side by side.

Now, refer to the following code snippet:

```
def dark_channel(im, sz):
    b, g, r = cv2.split(im)
    dc = cv2.min(cv2.min(r, g), b)
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (sz, sz))
    dark = cv2.erode(dc, kernel)
    return dark
```

```
```python
def estimate_atmospheric_light(im, dark):
    [h, w] = im.shape[:2]
    imsz = h * w
    numpx = int(max(np.floor(imsz / 1000), 1))
    darkvec = dark.reshape(imsz, 1)
    imvec = im.reshape(imsz, 3)
    indices = darkvec.argsort()
    indices = indices[imsz-numpx:]
    atmsum = np.zeros([1, 3])
    for ind in indices:
        atmsum = atmsum + imvec[ind]
    A = atmsum / numpx
    return A


def dehaze(im, A, omega=0.95, t0=0.1):
    im3 = np.empty(im.shape, im.dtype)
    for ind in range(0, 3):
        im3[:, :, ind] = im[:, :, ind] / A[0, ind]
    transmission = 1 - omega * dark_channel(im3, 15)
    t = cv2.max(transmission, t0)
    J = np.empty(im.shape, im.dtype)
    for ind in range(0, 3):
        J[:, :, ind] = (im[:, :, ind] - A[0, ind]) / t + A[0, ind]
    return J


img = plt.imread('images/hazy.jpg')
A = estimate_atmospheric_light(img, dark_channel(img, 15))
dehazed_img = dehaze(img, A)
plt.imshow(np.hstack((img, dehazed_img))), plt.axis('off')
```
```

If you run the above code snippet, you should obtain the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_370_0.png)

Figure 7.37 – Dehazing with dark channel prior

The method works best on natural outdoor images with distinct haze. Indoor images or those without strong haze may not benefit. Refining the transmission map (e.g., with guided filtering) can significantly improve visual quality.

## Super-resolution with enhanced deep residual networks

Image **super-resolution (SR)** is the task of reconstructing a **high-resolution (HR)** image from its **low-resolution (LR)** counterpart. It is crucial for applications like medical imaging (e.g., zooming in on scans), satellite and aerial imagery, surveillance and forensics, and enhancing consumer images and videos. Traditional methods like bicubic interpolation fail to preserve textures and sharp edges. Deep learning–based approaches, such as **Enhanced Deep Residual Networks (EDSR)**, provide a significant leap forward.

Given a low-resolution image $I_{LR}$, we aim to estimate its high-resolution counterpart $I_{SR}$, such that $I_{SR} \approx I_{HR}$. The network learns a mapping function $f_\theta$ parameterized by deep network weights:

$I_{SR} = f_\theta(I_{LR})$

## Architecture of EDSR

EDSR is an improved version of the **SRResNet** model, which builds on residual learning. Its architecture includes:

- **Feature extraction layer:** $F_0 = Conv(I_{LR})$
- **Residual blocks (×N):** Each block contains:
- Conv → ReLU → Conv
- **Residual skip connection:**
  
  $F_{i+1} = F_i + B(F_i)$
- **Long skip connection:**
  
  $F_{res} = F_0 + Conv(F_N)$
- **Upsampling layers:** Uses sub-pixel convolution or pixel shuffle [Shi et al., 2016]
- **Output layer:** Final convolution to get the RGB SR image

Now, let’s deep-dive into Python implementation and understand how the following code works. Here’s a concise summary of what the following code does, broken into key steps and functions:

- Loads and normalizes input image using Lambda(normalize) to center pixel values
- Defines residual blocks with res_block() using two convolutional layers and a skip connection
- Stacks multiple residual blocks (e.g., 16 blocks) and adds a long skip connection
- Applies upsampling via pixel_shuffle() in the upsample() function for 2×, 3×, or 4× resolution
- Builds the full model using edsr() that returns high-resolution output
- Loads pretrained weights from 'models/weights-edsr-16-x4.h5'
- Reads low-resolution input image using PIL.Image.open()
- Performs baseline upscaling via bicubic interpolation using PIL.Image.resize()
- Applies EDSR model inference using resolve_single() and resolve()
- Displays LR, bicubic, and EDSR outputs side by side using matplotlib.pyplot

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

```
```python
def resolve_and_plot(model_pre_trained, lr_image_path):
    im_lr = Image.open(lr_image_path)
    lr = np.array(im_lr)[...,:3]
    sr_ci = np.array(im_lr.resize((lr.shape[1]*4, lr.shape[0]*4),
                                  Image.BICUBIC))[...,:3]
    sr_pt = resolve_single(model_pre_trained, lr)
    model_name = model_pre_trained.name.upper()
    # Plot low-res image (LR), sr_ci and sr_pt side-by-side here

weights_dir = 'models/'
lr_image_path = 'images/butterfly.png'
# EDSR baseline as described in the EDSR paper (1.52M parameters)
edsr_pre_trained = edsr(scale=4, num_res_blocks=16)
edsr_pre_trained.load_weights(os.path.join(weights_dir, \
                               'models/weights-edsr-16-x4.h5'))
resolve_and_plot(edsr_pre_trained, lr_image_path)
```
```

If you run the above code snippet, you should obtain the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_373_0.png)

*Figure 7.38 – Image super-resolution with bicubic interpolation vs. a pretrained EDSR model*

The following table summarizes and compares the above images:

| Method | Description |
|---|---|
| Low-Resolution (LR) | Input image |
| Bicubic Interpolation | Baseline upscaling |
| EDSR Output | Deep-learning super-resolution |

*Table 7.8 – A comparison of the various methods*

The difference is visually striking. EDSR produces sharper edges, better textures, and natural colors compared to naive interpolation.

## Summary

In this chapter, we explored a wide range of image enhancement techniques, from point-wise intensity transformations (e.g., negative, power-law, contrast stretching) and histogram-based methods (e.g., equalization and matching) to noise reduction using linear (mean, Gaussian) and nonlinear filters (median, bilateral, non-local means). We also introduced fuzzy logic-based enhancement, wavelet-domain fusion, and deep learning-based methods for low-light correction, denoising, dehazing, and super-resolution. Readers should now be able to implement these techniques in Python, apply them to improve image quality, and interactively explore their effects using the Streamlit app. In the following chapter, we shall continue discussing more image enhancement techniques based on image derivatives and gradients.

## Get this book’s PDF version and more

Scan the QR code (or go to packtpub.com/unlock). Search for this book by name, confirm the edition, and then follow the steps on the page.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_374_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_374_1.png)

Note: Keep your invoice handy. Purchases made directly from Packt don’t require an invoice.
