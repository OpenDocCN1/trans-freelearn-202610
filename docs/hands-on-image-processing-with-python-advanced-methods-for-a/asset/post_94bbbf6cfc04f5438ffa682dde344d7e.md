
## Image Restoration: Inverse Problems in Imaging

Image restoration is a fundamental problem in image processing and computer vision. Its primary goal is to reconstruct or recover a high-quality image from an observed, degraded version. Degradations may arise due to limitations in imaging systems, environmental conditions, or transmission channels, and often manifest as blur, noise, low resolution, or missing data. The ability to restore images has immense practical importance, influencing domains such as medical diagnostics, astronomy, remote sensing, photography, and surveillance.

Unlike **image enhancement**, which subjectively improves visual quality without necessarily modeling the source of degradation, **image restoration** relies on a mathematical model of the degradation process. By incorporating knowledge of how the original image was altered, restoration methods aim to “invert” the process and approximate the true, undegraded image.

In this chapter, we will study a progression of techniques for image restoration, beginning with classical analytical methods, moving through sparse representations, and finally arriving at modern deep learning-based approaches. Specifically, we will cover the following key topics:

- Image restoration as an inverse problem
- Classical approaches to image deconvolution
- Image restoration with sparse representations
- Image restoration with deep learning
- Stable Diffusion-based inpainting with DeepFill

### Technical requirements

Follow along with the Jupyter notebook (and run the necessary code snippets) while going through different sections of this chapter. Here is the GitHub link to the notebook corresponding to this chapter: https://github.com/PacktPublishing/Hands-On-Image-Processing-and-Computer-Vision-with-Python-Second-Edition. It’s necessary to have the right versions of the libraries installed prior to running the code in order for the code to run seamlessly. If there are version mismatches, the code may need a little bit of tweaking here and there, based on whether or not some features/functions from the libraries get removed or become deprecated.

Note that the implementations in this chapter are based on Python (≥3.8) and require standard scientific computing libraries along with deep learning frameworks. Classical methods use numpy and scikit-image, while deep learning sections additionally depend on torch, diffusers, basicsr, simple_lama_inpainting, gradio, and pyyaml (version compatibility with recent stable releases is recommended). A CUDA-enabled GPU is recommended for efficient execution of deep learning models, with ~6 GB VRAM sufficient for inference under reduced resolution and batch size, while ≥8 GB VRAM ensures smoother performance. For reproducibility, several sections also require cloning external repositories (e.g., NAFNet and InstructIR), for which setup instructions and dependency installation steps are provided inline.

### Image restoration as an inverse problem

The essence of image restoration lies in treating it as an **inverse problem** in imaging. Mathematically, the observed degraded image *g(x, y)* can be expressed as:

$$g(x,y) = f(x,y) * h(x,y) + n(x,y)$$

where:

- *f(x, y)*: the unknown original image,
- *h(x, y)*: the degradation function or point spread function (PSF),
- *n(x, y)*: additive noise,
- *g(x, y)*: the observed degraded image,
- *\**: convolution operator.

Restoration then seeks an estimate $\hat{f}(x,y)$ of the original image $f(x,y)$. However, because of noise and information loss, the problem is ill-posed in the sense of Hadamard:

- 1. A unique solution may not exist.
- 2. The solution may not depend continuously on the input (instability).
- 3. Additional constraints or priors are required to regularize the solution.

Thus, image restoration is not a straightforward inversion problem but one requiring careful modeling, optimization, and prior knowledge. The following figure shows the basic image degradation model:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_429_0.png)

*Figure 9.1 – Image restoration process*

The study of image restoration has deep roots:

- **Early analytical methods:** Techniques such as Wiener filtering and inverse filtering were developed in the context of communication theory, providing closed-form solutions for deblurring under Gaussian noise.
- **Regularization-based methods:** Tikhonov regularization, total variation minimization, and Bayesian frameworks introduced stability into ill-posed problems.
- **Sparse representations and dictionary learning:** Restoration benefited from advances in sparse coding and wavelet-domain denoising, enabling sharper reconstructions.
- **Deep learning approaches:** Convolutional neural networks (CNNs), generative models, and transformers have revolutionized restoration, achieving state-of-the-art performance in denoising, deblurring, super-resolution, and inpainting.

#### Core tasks in image restoration

Several canonical problems form the foundation of image restoration research:

- **Image denoising:** Removing noise while preserving fine structures. Early approaches used linear filters; modern methods exploit non-local similarities and deep neural networks.
- **Image deblurring:** Reversing the effects of motion blur or defocus blur. Solutions range from classical deconvolution to blind deblurring with learned priors.
- **Image inpainting:** Filling in missing or corrupted regions in an image. Modern techniques employ patch-based synthesis and deep generative models.
- **Dehazing and deraining:** Removing atmospheric haze, fog, or rain streaks to recover visibility and contrast. Physics-based models and CNNs dominate recent progress.
- **Super-resolution:** Reconstructing high-resolution images from low-resolution inputs, leveraging priors, sparse coding, and deep super-resolution networks.

#### Denoising vs. deblurring

In case of image denoising, the degradation is caused primarily by additive random noise (e.g., Gaussian sensor noise, Poisson shot noise).

- **Model:** $g(x, y) = f(x, y) + \eta(x, y)$
- **Goal:** Suppress noise while preserving details (edges, textures).
- **Typical approaches:** Gaussian smoothing, Non-Local Means, BM3D, deep denoising networks (DnCNN, NAFNet).

In case of image deblurring, the degradation is caused by convolution with a blur kernel (motion blur, defocus blur, atmospheric turbulence).

- **Model:** $g(x, y) = (h * f)(x, y) + \eta(x, y)$
- **Goal:** Undo blurring by deconvolution while being robust to noise.
- **Typical approaches:** Inverse filtering, Wiener filter, Richardson–Lucy deconvolution, deep blind deblurring networks.

The key difference is that denoising deals with *stochastic noise removal*, while deblurring focuses on *reversing deterministic convolution*.

#### Applications

Image restoration has real-world significance:

- **Medical imaging:** Enhancing MRI or CT scans for accurate diagnosis.
- **Astronomy and remote sensing:** Correcting atmospheric distortions in telescope and satellite images.
- **Surveillance and forensics:** Improving degraded footage for recognition and analysis.
- **Photography:** Reducing noise and blur in low-light or handheld captures.

Since blur is one of the most common and mathematically tractable forms of image degradation, we begin with classical image deconvolution techniques that aim to recover sharp images from blurred observations.

### Classical approaches to image deconvolution

Blurring is one of the most common forms of degradation in imaging systems. It arises from optical imperfections, motion during exposure, or out-of-focus capture. Mathematically, blurring can be modeled as a convolution of the original image $f(x, y)$ with a point spread function (PSF) $h(x, y)$, possibly followed by the addition of noise $n(x, y)$:

$g(x, y) = f(x, y) * h(x, y) + n(x, y)$

The process of deconvolution attempts to reverse this operation, i.e., to recover an estimate of $f(x, y)$ from the observed blurred image $g(x, y)$.
