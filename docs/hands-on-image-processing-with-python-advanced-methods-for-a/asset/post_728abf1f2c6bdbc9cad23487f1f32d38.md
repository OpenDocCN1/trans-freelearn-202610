
![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_356_0.png)

> Figure 7.33 – Denoising with BM3D with different noise variances

BM3D provides state-of-the-art quality in denoising, preserves edges, fine details, and textures, and handles high and low noise levels effectively. However, it is computationally intensive compared to simpler methods and requires accurate noise level estimation.

## Wavelet-based image fusion and enhancement

Image fusion combines multiple images of the same scene to produce a single enhanced image containing more meaningful information. It is particularly useful in applications such as medical imaging, remote sensing, and exposure fusion in photography. Wavelet transform-based image fusion is a widely used multi-resolution approach that decomposes the input images into frequency sub-bands and combines them using fusion rules. The inverse transform is then used to reconstruct the fused image. This section provides a detailed understanding of wavelet-based image fusion along with mathematical explanation, intuition, and a Python implementation.

## Discrete wavelet transform

The discrete wavelet transform (DWT) decomposes an image into approximation and detail coefficients:

- Approximation coefficients (LL): Low-frequency components
- Detail coefficients (LH, HL, HH): High-frequency components in horizontal, vertical, and diagonal directions, respectively

Let f(x, y) be an input image of size M × N. A one-level 2D DWT is applied as follows:

### Step 1: Row-wise filtering and downsampling

Each row of the image is convolved with:

- A low-pass filter (h_L[n]),
- A high-pass filter (h_H[n])

The results are then downsampled by 2 (keeping every second sample):

```
L_r(x, y) = (f * h_L)(x, y) ↓₂
H_r(x, y) = (f * h_H)(x, y) ↓₂
```

Here, * denotes 1D convolution along rows.

### Step 2: Column-wise filtering and downsampling

The intermediate results L_r and H_r are then convolved column-wise with the same pair of filters h_L^T and h_H^T, followed by downsampling by 2:

```
LL(x, y) = (L_r * h_L^T)(x, y) ↓₂
LH(x, y) = (L_r * h_H^T)(x, y) ↓₂
HL(x, y) = (H_r * h_L^T)(x, y) ↓₂
HH(x, y) = (H_r * h_H^T)(x, y) ↓₂
```

Thus, the image is decomposed into four sub-bands: LL, LH, HL, and HH.

Here, h_L^T and h_H^T denote the transpose of the filters (for column-wise convolution), and ↓₂ represents downsampling by a factor of two in the respective direction.

Common wavelet types include:

- **Haar (db1)**: The simplest orthogonal wavelet. It performs basic averaging and differencing.
- **Daubechies (dbN)**: A family of orthogonal wavelets with more vanishing moments for smoother approximations.

Let I₁ and I₂ be two input images. Applying a 2D wavelet decomposition, a fusion rule is applied to the coefficients. Fusion rules include:

- **Mean**: average coefficients
- **Max**: retain stronger features
- **Min**: retain weaker features

Next, reconstruct the fused image using inverse DWT.

The next code demonstrates image fusion using DWT with three different fusion strategies: mean, max, and min. Below is a detailed explanation of each part of the code, its purpose, and how it works:

- **Pywavelets (Pywt)**: The Python wrapper for wavelet transforms; it enables wavedec2 (2D DWT), and waverec2 (inverse 2D DWT).
- **cv2**: OpenCV, used for image reading, resizing, and grayscale conversion.
- **Numpy**: Efficient array operations.
- **Matplotlib.pylab**: For image display and plotting.

The function fuse_coefficients() defines how two wavelet coefficient matrices are fused.

Methods include the following:

- mean: Takes the average of the two coefficients: Cf = (C1+C2)/2
- min: Element-wise minimum, useful for suppressing bright noise: Cf = min(C1,C2)
- max: Retains maximum features (e.g., strong edges): Cf = max(C1,C2)

The function fuse_images() implements the image fusion pipeline.

Image resizing ensures both images are the same size (necessary for DWT). 2D Wavelet decomposition performs multi-level DWT using a selected wavelet (default db1, i.e., Haar). It returns:

- coeffs[0]: Approximation coefficients (low-pass)
- coeffs[1:]: Tuples of detail coefficients:
  - **Horizontal (LH)**
  - **Vertical (HL)**
  - **Diagonal (HH)**

Each level decomposes the image into fusion of coefficients: If i == 0, fuse the approximation coefficients; else, fuse the detail coefficients. These preserve different directional features (e.g., vertical edges in HL). Reconstruction performs inverse wavelet reconstruction:

$I_{fused} = IDWT2(Af, Hf, Vf, Df)$

Normalization ensures the final image is scaled to the 8-bit range [0, 255], suitable for display.

Now, refer to the next code snippet:

```python
import pywt, cv2
import numpy as np
import matplotlib.pylab as plt

def fuse_coefficients(coeff1, coeff2, method='mean'):
    if method == 'mean':
        return (coeff1 + coeff2) / 2
    elif method == 'min':
        return np.minimum(coeff1, coeff2)
    elif method == 'max':
        return np.maximum(coeff1, coeff2)
    else: raise ValueError("Unsupported fusion method")

def fuse_images(im1, im2, fusion_method, wavelet='db1'): # Daubechies wavelet
    im2 = cv2.resize(im2, (im1.shape[1], im1.shape[0])) # Resize im2 to match im1
    coeffs1 = pywt.wavedec2(im1, wavelet) # Wavelet decomposition
    coeffs2 = pywt.wavedec2(im2, wavelet) # Wavelet decomposition
    # Fuse wavelet coefficients
    fused_coeffs = []
    for i in range(len(coeffs1)):
        if i == 0:
            fused_coeffs.append(fuse_coefficients(coeffs1[0], coeffs2[0], fusion_method)) # Approximation coefficients
        else: # Detail coefficients (horizontal, vertical, diagonal)
            c1 = fuse_coefficients(coeffs1[i][0], coeffs2[i][0], fusion_method)
            c2 = fuse_coefficients(coeffs1[i][1], coeffs2[i][1], fusion_method)
            c3 = fuse_coefficients(coeffs1[i][2], coeffs2[i][2], fusion_method)
            fused_coeffs.append((c1, c2, c3))
    fused_image = pywt.waverec2(fused_coeffs, wavelet) # Reconstruct fused image
    fused_image = 255 * (fused_image - fused_image.min()) / (fused_image.max() - fused_image.min()) # Normalize to 8-bit range
    return fused_image.astype(np.uint8)

im1 = cv2.imread('images/mask.png', cv2.IMREAD_GRAYSCALE) # load images
im2 = cv2.imread('images/dust.png', cv2.IMREAD_GRAYSCALE)
plt.gray() # Show images
plt.imshow(np.hstack((im1, im2, \
    cv2.resize(fuse_images(im1, im2, 'mean'), (im1.shape[1],
        im1.shape[0])),
    cv2.resize(fuse_images(im1, im2, 'max'), (im1.shape[1],
        im1.shape[0])),
    cv2.resize(fuse_images(im1, im2, 'min'), (im1.shape[1],
        im1.shape[0])))))
```

If you run the above code snippet, you should obtain the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_360_0.png)

> Figure 7.34 – Image fusion with wavelets

Use cases of DWT for fusion include:

- Localized both in space and frequency
- Captures edges, textures, and smooth areas effectively
- Flexible fusion rules for different applications (e.g., visual contrast, noise suppression)

## Image enhancement with deep learning models

With the advent of deep learning, image enhancement has transcended the limitations of traditional algorithms, enabling data-driven restoration and perceptual quality improvement. In this section, we explore state-of-the-art deep models, such as Zero-DCE, MAXIM, DCP, and EDSR, demonstrating their power in low-light enhancement, denoising, dehazing, and super-resolution. We shall focus solely on using pretrained deep learning models for image enhancement, while training models on custom datasets will be covered in later chapters.

## Low-light image enhancement with Zero-DCE

Low-light image enhancement is a critical pre-processing step in many computer vision tasks, especially in autonomous driving, surveillance, medical imaging, and night photography. Traditional enhancement methods, such as histogram equalization, gamma correction, and Retinex-based techniques, often fall short when applied to severely underexposed images due to noise amplification, color distortion, or over-saturation.

In 2020, Guo et al. introduced an innovative learning-based technique called Zero-Reference Deep Curve Estimation (Zero-DCE), which removes the need for paired training data (i.e., low-light and ground-truth normal-light image pairs) and instead learns to enhance images directly in an unsupervised manner. This section explains the theoretical foundation, intuition, and practical implementation of Zero-DCE using PyTorch.

### Enhancement function: iterative curve estimation

The core of Zero-DCE lies in learning a set of enhancement curves to adjust pixel intensities iteratively.

Each pixel value x ∈ [0,1] is enhanced using:

$x_{t+1} = x_t + α_t · x_t · (1 − x_t)$

This expression can be derived from a general lighting adjustment intuition: x_t · (1 − x_t) is maximum at mid-intensity (x=0.5), allowing the enhancement to adapt non-linearly. α_t ∈ [−1,1] is learned per pixel and iteration by a neural network.

Iterative application of this curve refines the brightness gradually. In practice, the authors found a symmetric variant equally effective; this form avoids the multiplication with x · (1 − x), but the effect is similar due to its symmetry.

Zero-DCE does not require ground truth images. Instead, it relies on self-regularized loss terms:

- Spatial consistency Loss L_spa: encourages structural consistency between input and enhanced images.
- Exposure control Loss L_exp: guides the average brightness to a reference level (typically 0.6).
- Color constancy Loss L_col: ensures balanced R, G, and B channels.
- Illumination smoothness Loss L_tv: regularizes the predicted illumination maps.
