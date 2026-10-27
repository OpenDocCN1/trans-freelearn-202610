Let f(x, y) be an input image of size M × N. A one-level 2D DWT is applied as follows:

# Step 1: Row-wise filtering and downsampling: Each row of the image is convolved with:

+   - A low-pass filter (h_L[n]),
+   - A high-pass filter (h_H[n])

The results are then downsampled by 2 (keeping every second sample):

```
L_r(x, y) = (f * h_L)(x, y) ↓₂

H_r(x, y) = (f * h_H)(x, y) ↓₂
```

Here, * denotes 1D convolution along rows.

# Step 2: Column-wise filtering and downsampling: The intermediate results (L_r) and (H_r) are then convolved column-wise with the same pair of filters (h_L^T) and (h_H^T), followed by downsampling by 2:

```
LL(x, y) = (L_r * h_L^T)(x, y) ↓₂

LH(x, y) = (L_r * h_H^T)(x, y) ↓₂

HL(x, y) = (H_r * h_L^T)(x, y) ↓₂

HH(x, y) = (H_r * h_H^T)(x, y) ↓₂
```

Thus, the image is decomposed into four sub-bands: LL LH, HL and HH.

Here, h_L^T and h_H^T denote the transpose of the filters (for column-wise convolution), and ↓₂ represents downsampling by a factor of two in the respective direction.

Common wavelet types include:

+   - **Haar (db1)**: The simplest orthogonal wavelet. It performs basic averaging and differencing.
+   - **Daubechies (dbN)**: A family of orthogonal wavelets with more vanishing moments for smoother approximations.

Let I₁ and I₂ be two input images. Applying a 2D wavelet decomposition, a fusion rule is applied to the coefficients. Fusion rules include:

+   - **Mean**: average coefficients
+   - **Max**: retain stronger features
+   - **Min**: retain weaker features